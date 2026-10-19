> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Apache e PHP-FPM — ambiente de manutenção

## Por que usar este ambiente

Aqui você confere o encaminhamento do Apache, a interpretação de `.htaccess` e a integração com PHP-FPM. O servidor embutido do capítulo anterior é suficiente para vários exercícios PHP, mas não substitui esses testes.

O exemplo usa uma cópia em `/srv/galastri-dev` e porta local `8081`. Isso evita depender das permissões da pasta pessoal e não exige trocar o site padrão do computador.

## 1. Preparar a cópia

No Ubuntu:

```bash
sudo install -d -o "$(id -un)" -g www-data -m 0750 /srv/galastri-dev
git clone https://github.com/andregalastri/galastri-framework-2.git /srv/galastri-dev
cd /srv/galastri-dev
composer install
```

A pasta de destino precisa estar vazia no clone inicial. Se já houver uma cópia sua nesse caminho, confira-a e use-a; não a apague para repetir o comando.

Prepare somente a pasta que precisa de escrita:

```bash
sudo chgrp www-data logs
sudo chmod 2770 logs
```

Os arquivos normais precisam ser legíveis e os diretórios atravessáveis pelo usuário do FPM. Não aplique `chmod -R 777` ao projeto. O usuário do serviço normalmente é `www-data` no Ubuntu; confira o pool se sua instalação tiver sido personalizada.

## 2. Instalar serviços

```bash
sudo apt install apache2 php-fpm
sudo a2enmod proxy_fcgi setenvif rewrite
```

Para esta base Ubuntu 24.04, o serviço é `php8.3-fpm`. Confira antes de copiar a configuração:

```bash
ls /run/php/
systemctl status php8.3-fpm
```

Se usar outra versão, substitua **o nome do serviço e o socket** em todos os trechos. Instalar PHP CLI não instala necessariamente o FPM correspondente.

## 3. Configurar a porta local

Crie `/etc/apache2/conf-available/galastri-dev-port.conf`:

```apache
Listen 127.0.0.1:8081
```

Isso acrescenta uma porta local. Não há necessidade de mudar `ports.conf` ou remover outros sites.

## 4. Criar o VirtualHost

Crie `/etc/apache2/sites-available/galastri-dev.conf`:

```apache
<VirtualHost 127.0.0.1:8081>
    ServerName localhost
    DocumentRoot /srv/galastri-dev/public

    <Directory /srv/galastri-dev/public>
        Require all granted
        AllowOverride All
        Options -Indexes
        DirectoryIndex index.php

        <FilesMatch "\.php$">
            SetHandler "proxy:unix:/run/php/php8.3-fpm.sock|fcgi://localhost"
        </FilesMatch>
    </Directory>

    <Proxy "fcgi://localhost">
        ProxySet flushpackets=on
    </Proxy>

    ErrorLog ${APACHE_LOG_DIR}/galastri-dev-error.log
    CustomLog ${APACHE_LOG_DIR}/galastri-dev-access.log combined
</VirtualHost>
```

`DocumentRoot` contém somente `public`. `AllowOverride All` permite que o Apache aplique as diretivas do `.htaccess` do projeto. `SetHandler` envia PHP ao socket definido. `flushpackets=on` participa da verificação de streaming no caminho FastCGI; o capítulo de streaming explica as demais camadas de buffer.

## 5. Ativar e conferir antes de recarregar

```bash
sudo a2enconf galastri-dev-port
sudo a2ensite galastri-dev
sudo apache2ctl configtest
sudo systemctl enable --now php8.3-fpm
sudo systemctl reload apache2
curl -i http://127.0.0.1:8081/
```

Se Apache ainda não estiver ativo, use `sudo systemctl start apache2` antes do reload. Em WSL, confirme que sua distribuição utiliza systemd; em instalações sem ele, os comandos de serviço precisam ser adaptados ao gerenciador disponível.

Resultado esperado: resposta da aplicação inicial. Uma URL inexistente deve chegar ao Router e produzir o tratamento de erro do framework; favicon ausente tem uma regra especial que retorna 404 sem executar o bootstrap.

## O comportamento específico do .htaccess

A regra final exclui **diretórios existentes**. Ela não usa a condição habitual `!-f`: arquivos existentes, em geral, também seguem para `index.php`, exceto os casos tratados antes. Isso permite que o framework decida como entregá-los. Não copie uma configuração genérica que libere todos os arquivos e espere comportamento idêntico.

O arquivo também desativa listagem, ETag automático e gzip pelo `mod_deflate` quando disponível. Essas opções têm efeito no Apache, não no servidor embutido.

## Diagnóstico

| Sintoma | O que conferir |
| --- | --- |
| `Connection refused` | Apache ativo, `Listen` carregado e porta correta. |
| `503` no PHP | Serviço FPM, socket e permissões de acesso ao socket. |
| `403` na pasta | Permissões de travessia, `Require` e caminho do DocumentRoot. |
| Funciona em `/`, mas outras rotas não | `mod_rewrite`, `.htaccess` presente e `AllowOverride`. |
| Não cria log do framework | Escrita em `logs/` pelo usuário do FPM. |
| Progresso chega todo ao final | Buffer PHP/FPM, FastCGI, compressão e cliente. |

Próximo: [HTTPS local](/andregalastri/galastri-framework-2/wiki/HTTPS-local) para experimentar autenticação sem alterar a política de cookies do framework.

## Onde conferir no código

- [`public/.htaccess`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/public/.htaccess)
- [`public/index.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/public/index.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
