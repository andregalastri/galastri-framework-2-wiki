> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Nginx e PHP-FPM — configuração equivalente de desenvolvimento

## O fragmento do projeto não é um servidor completo

`public/nginx.conf` contém diretivas de roteamento. Ele não declara `server`, `listen`, `root` nem o socket PHP-FPM. Incluí-lo sem essas peças não cria uma instalação funcional.

O exemplo usa a mesma cópia `/srv/galastri-dev` e outra porta local, `8082`. Siga a preparação de arquivos e permissões do capítulo Apache, mesmo que opte por instalar apenas Nginx.

## 1. Instalar

```bash
sudo apt install nginx php-fpm
ls /run/php/
```

O exemplo adota `/run/php/php8.3-fpm.sock`, da base Ubuntu 24.04. Ajuste o caminho se a versão instalada for outra.

## 2. Configurar

Crie `/etc/nginx/sites-available/galastri-dev`:

```nginx
server {
    listen 127.0.0.1:8082;
    server_name localhost;
    root /srv/galastri-dev/public;

    include /srv/galastri-dev/public/nginx.conf;

    location = /index.php {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
        fastcgi_buffering off;
    }

    location ~ \.php$ {
        return 404;
    }

    access_log /var/log/nginx/galastri-dev-access.log;
    error_log /var/log/nginx/galastri-dev-error.log;
}
```

A localização exata de `index.php` encaminha somente a entrada ao FPM; a localização genérica evita executar outro arquivo PHP colocado sob a raiz pública. No Ubuntu, `fastcgi_params` é o arquivo de parâmetros fornecido pelo pacote Nginx. `SCRIPT_FILENAME` informa ao FPM qual arquivo abrir.

O arquivo do projeto já contém `location /`; não crie outro bloco com o mesmo endereço. A diretiva `include` incorpora o texto da configuração no ponto em que foi declarada, não faz uma requisição HTTP ao arquivo.

## 3. Ativar

```bash
sudo ln -s /etc/nginx/sites-available/galastri-dev /etc/nginx/sites-enabled/galastri-dev
sudo nginx -t
sudo systemctl enable --now php8.3-fpm
sudo systemctl start nginx
sudo systemctl reload nginx
curl -i http://127.0.0.1:8082/
```

Crie o link somente na primeira ativação; se ele já existir, confira o destino. `nginx -t` precisa passar antes do reload. Não remova outros sites do computador para liberar esta porta local.

## Entender try_files neste projeto

O fragmento usa:

```nginx
location / {
    try_files $uri/ /index.php?$query_string;
}
```

O teste `$uri/` procura um diretório. Quando ele não resolve, a requisição segue internamente para `index.php`, preservando a query string. O trecho não é o exemplo genérico `try_files $uri $uri/ ...`, que também libera arquivos físicos. Essa diferença se aproxima da decisão do `.htaccess` de preservar diretórios e encaminhar os demais caminhos.

Um `include` de configuração não garante sozinho que Apache e Nginx processem todos os casos de forma idêntica. Compare barra final, diretórios, favicon e arquivos com casos HTTP reais.

## Streaming e diagnóstico

`fastcgi_buffering off` reduz uma camada de acúmulo. `Json::stream()` também emite `X-Accel-Buffering: no`, encerra buffers removíveis e chama `flush()`. Proxies adicionais e o cliente podem continuar agrupando bytes.

Um `502` normalmente exige conferir o FPM e o socket antes de examinar o Router. Um 404 da aplicação, por sua vez, indica que o PHP foi executado e a investigação pode avançar para a árvore de rotas.

## Onde conferir no código

- [`public/nginx.conf`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/public/nginx.conf)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
