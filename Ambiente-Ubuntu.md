> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Ubuntu — instalação e primeira execução

## Base deste roteiro

Os comandos usam Bash no Ubuntu 24.04 LTS. No Windows, execute-os dentro do Ubuntu preparado no WSL 2. O PHP padrão dessa distribuição é da série 8.3; os nomes genéricos abaixo acompanham a versão fornecida pela distribuição.

## 1. Instalar ferramentas e extensões

```bash
sudo apt update
sudo apt install git curl unzip composer php-cli php-mbstring php-gd php-xml php-curl php-sqlite3 php-mysql
```

O conjunto inclui SQLite para os laboratórios e o driver MySQL para quando ele for necessário. `php-xml`, `php-curl` e `unzip` também tornam o uso do Composer mais direto. Fileinfo e sessões normalmente vêm nos pacotes básicos; confira efetivamente o carregamento.

```bash
php -v
php --ini
php -m
composer --version
git --version
```

Se houver várias instalações PHP, `command -v php` e `php --ini` mostram qual CLI está sendo executada. O PHP-FPM pode usar outro `php.ini`; isso será conferido no capítulo de servidor.

## 2. Clonar em uma pasta de trabalho

```bash
mkdir -p ~/projetos
cd ~/projetos
git clone https://github.com/andregalastri/galastri-framework-2.git
cd galastri-framework-2
git log -1 --oneline
```

O clone traz o estado atual da branch padrão. Para reproduzir exatamente esta edição, use uma cópia de estudo e consulte o SHA em [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao). Um problema observado num commit posterior precisa ser comparado com as mudanças intermediárias.

## 3. Instalar o autoload

```bash
composer install
composer check-platform-reqs
```

`install` usa o lock existente; `update` é uma operação diferente, que resolve versões e pode mudar o lock. Não use `--ignore-platform-reqs` para esconder uma extensão faltante.

O projeto não depende de um pacote de framework externo: o próprio namespace `galastri\` aponta para a pasta do repositório. Mesmo assim, o bootstrap inclui `vendor/autoload.php`, portanto a geração do autoload continua necessária.

## 4. Conferir as extensões utilizadas

Crie **`teste.php` na raiz local**:

```php
<?php
$extensions = ['gd', 'mbstring', 'fileinfo', 'session', 'PDO', 'pdo_sqlite'];
$failed = false;

foreach ($extensions as $extension) {
    if (extension_loaded($extension)) {
        echo 'OK: '.$extension.PHP_EOL;
    } else {
        echo 'FALTOU: '.$extension.PHP_EOL;
        $failed = true;
    }
}

if ($failed) {
    exit(1);
}

echo 'Ambiente básico disponível.'.PHP_EOL;
```

Execute:

```bash
php teste.php
```

Este teste verifica a CLI. Não comprova que o FPM carregue as mesmas extensões ou que GD tenha todo recurso opcional de renderização; para File, confira também `gd_info()` e o suporte FreeType.

## 5. Abrir a aplicação

Na raiz do projeto:

```bash
php -S 127.0.0.1:8080 -t public public/index.php
```

Mantenha o terminal aberto. Em outro terminal:

```bash
curl -i http://127.0.0.1:8080/
```

Abra a mesma URL no navegador. A aplicação inicial deve gerar a página de exemplo. Encerre **esse servidor** com `Ctrl+C` no terminal onde ele foi iniciado.

O último argumento, `public/index.php`, é o roteador do servidor embutido. Sem ele, não estamos reproduzindo as regras do Apache. Este modo também não aplica as regras de favicon, compressão e barra final de `.htaccess`; para conferi-las, use o servidor correspondente.

## 6. Evitar incluir arquivos locais por acidente

Antes de preparar um commit de código, execute `git status --short`. O commit de referência não possui um `.gitignore` na raiz para todos os seus arquivos de laboratório.

Para excluir apenas na sua cópia, sem alterar o projeto compartilhado, adicione entradas ao arquivo `.git/info/exclude`:

```text
/vendor/
/teste.php
/var/
```

`logs/.gitignore` trata o conteúdo de logs, mas não substitui a conferência do status. Não coloque credenciais reais no arquivo de exemplo de banco.

## Resultado esperado

Você consegue executar PHP, carregar o autoload e abrir a página inicial. Se falhar, registre a versão PHP, extensões carregadas, commit e mensagem completa antes de investigar o núcleo.

Próxima etapa: [Apache e PHP-FPM](/andregalastri/galastri-framework-2/wiki/Ambiente-Apache) para um ambiente com as regras reais de reescrita, ou [Nginx e PHP-FPM](/andregalastri/galastri-framework-2/wiki/Ambiente-Nginx).

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`composer.lock`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.lock)
- [`public/index.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/public/index.php)
- [`public/.htaccess`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/public/.htaccess)
- [`app/config/database.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/config/database.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
