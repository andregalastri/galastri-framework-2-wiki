> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# MySQL e MariaDB — configuração do driver

## O papel do driver

`MysqlDatabase` prepara host, porta, banco, usuário, senha e opções. `connect()` monta o DSN `mysql:host=...;dbname=...`, acrescenta a porta configurada quando não vazia e entrega a criação de PDO à rotina comum da trait.

MariaDB pode ser acessado pelo driver PDO MySQL correspondente. O módulo não converte SQL entre produtos nem garante comportamento idêntico de todas as versões.

## Instalar um serviço local opcional

Se você ainda não tem um banco local e quer fazer este exercício no Ubuntu, uma opção disponível nos repositórios da distribuição é MariaDB:

```bash
sudo apt install mariadb-server
sudo systemctl enable --now mariadb
sudo mariadb
```

O último comando abre o cliente administrativo local na configuração usual do pacote Ubuntu. Se o seu servidor já usa outra forma de autenticação administrativa, siga essa configuração em vez de redefini-la. Execute o SQL de criação abaixo nesse cliente e saia com `exit`.

A aplicação usa a conexão TCP de `127.0.0.1` pelo driver PDO MySQL. Esse laboratório não exige abrir o serviço para a rede.

## Perfil de desenvolvimento

Exemplo para `app/config/database.php` **somente na cópia local**, com valores fictícios:

```php
<?php
return [
    'desenvolvimento' => [
        'host' => '127.0.0.1',
        'port' => '3306',
        'database' => 'galastri_lab',
        'username' => 'galastri_lab',
        'password' => 'SUBSTITUA_PELA_SENHA_LOCAL',
        'options' => [
            PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES utf8mb4',
        ],
    ],
];
```

O perfil exige todas as chaves principais, inclusive `password` e `port`, mesmo que algum valor admitido seja vazio. `options` é opcional. O método `setPort()` do driver transforma a string da porta no trecho usado pelo DSN.

O DSN atual não possui uma chave própria de configuração `charset`. O exemplo usa uma opção PDO de inicialização para declarar a codificação. Não invente um `charset => 'utf8mb4'` no perfil e suponha que o driver o leia.

## Criação do banco e do usuário

Para um MariaDB local novo de estudo, os comandos abaixo são executados no cliente administrativo do banco, não num controller. Eles criam um banco e um usuário exclusivos de laboratório; não use nomes de produção.

```sql
CREATE DATABASE galastri_lab CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'galastri_lab'@'127.0.0.1' IDENTIFIED BY 'SUBSTITUA_PELA_SENHA_LOCAL';
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, INDEX, DROP ON galastri_lab.* TO 'galastri_lab'@'127.0.0.1';
```

Os privilégios de alteração estrutural atendem os exercícios locais. Uma aplicação de produção pode precisar de um conjunto mais restrito. Não é necessário abrir a porta 3306 na rede para conectar uma aplicação que roda na mesma máquina por `127.0.0.1`.

Se já existir um banco ou usuário com esses nomes, confira antes de executar; não substitua nem remova contas para reproduzir o exemplo.

## teste.php de conexão

Depois das constantes e autoload:

```php
$db = \galastri\modules\Database::createMysql('desenvolvimento');
$db->connect();
$db->query('SELECT 1 AS numero')->submit();
$result = $db->getResult();

if ((int) $result['data'][0]['numero'] !== 1) {
    throw new \RuntimeException('A consulta de conexão divergiu.');
}

echo 'OK: conexão MySQL.'.PHP_EOL;
$db->disconnect();
```

Esse teste verifica conexão e uma consulta; não comprova o comportamento de transações de todas as tabelas ou um schema real.

## Diagnóstico de conexão

`could not find driver` aponta primeiro para `pdo_mysql` da instalação PHP usada. `Access denied` exige conferir conta, host e senha. `Connection refused` exige conferir serviço e endereço antes de procurar defeito no encadeamento de queries.

Credenciais do perfil não devem aparecer numa Wiki pública nem em um relatório de erro compartilhado. Ao reportar, preserve a mensagem técnica necessária removendo os valores secretos.

## Onde conferir no código

- [`galastri/modules/database/MysqlDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/MysqlDatabase.php)
- [`galastri/modules/database/traits/CoreDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/traits/CoreDatabase.php)
- [`app/config/database.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/config/database.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
