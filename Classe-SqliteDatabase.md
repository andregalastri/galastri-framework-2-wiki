> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# SQLite — primeiro banco do laboratório

## 1. Um teste sem serviço externo

Crie **`teste.php` na raiz da cópia local**:

```php
<?php
require __DIR__.'/galastri/bootstrap/constants.php';
require __DIR__.'/vendor/autoload.php';

use galastri\modules\Database;

$db = Database::createSqlite();
$db->setLocation(':memory:')->connect();

$db->query('CREATE TABLE notas (id INTEGER PRIMARY KEY, titulo TEXT NOT NULL)')->submit();
$db->query('INSERT INTO notas (titulo) VALUES (:titulo)')
    ->bind(':titulo', 'Estudar o Router')
    ->submit();

$id = $db->getLastId();
$db->clearBind();
$db->query('SELECT id, titulo FROM notas WHERE id = :id')
    ->bind(':id', $id)
    ->submit();

$result = $db->getResult();

if (!$db->hasResult() || $result['data'][0]['titulo'] !== 'Estudar o Router') {
    throw new \RuntimeException('O resultado do banco divergiu.');
}

echo 'OK: inserção e consulta no SQLite.'.PHP_EOL;
$db->disconnect();
```

Cada operação com seus binds é submetida antes de trocar os parâmetros. `clearBind()` evita carregar `:titulo` para a consulta que usa apenas `:id`.

## 2. Persistência em arquivo

Para um banco local persistente, crie uma pasta `var/` fora de `public` e use `setLocation(PROJECT_DIR.'/var/laboratorio.sqlite')`.

SQLite precisa gravar tanto o arquivo quanto, em certas operações, arquivos auxiliares na pasta. No FPM, confira o usuário de execução e as permissões do diretório; um arquivo legível não significa que a conexão conseguirá escrever o journal.

Não exponha o arquivo de banco por uma rota File nem o inclua em commits do framework. Use dados de demonstração.

## 3. Perfil nomeado

No `app/config/database.php` da cópia local:

```php
<?php
return [
    'laboratorio' => [
        'location' => PROJECT_DIR.'/var/laboratorio.sqlite',
        'options' => [],
    ],
];
```

Depois:

```php
$db = Database::createSqlite('laboratorio');
$db->connect();
```

O nome seleciona uma entrada do arquivo; não é o nome da conexão PDO. O construtor configura o objeto, mas a conexão só é aberta em `connect()`.

## Implementação específica

O driver usa `DRIVER = 'sqlite'` e exige a chave `location` no perfil. Sem um nome de perfil, retorna um objeto que pode ser configurado por setters. `connect()` cria PDO com DSN `sqlite:` seguido da localização e passa as opções ao construtor.

A trait ativa exceções de PDO e desabilita prepares emulados na conexão. O SQL permitido continua dependendo do SQLite instalado, não de uma tradução automática entre dialetos.

## O que conferir ao alterar o driver

Teste `:memory:`, arquivo existente, pasta sem escrita e reconexão indevida. Um teste exclusivamente em memória não verifica permissões reais nem persistência no disco.

## Onde conferir no código

- [`galastri/modules/database/SqliteDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/SqliteDatabase.php)
- [`galastri/modules/database/traits/CoreDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/traits/CoreDatabase.php)
- [`app/config/database.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/config/database.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
