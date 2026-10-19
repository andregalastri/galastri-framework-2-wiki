> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Database — fábrica de conexões e execução adiada

## A fábrica é pequena; o comportamento está nos objetos

`Database::createMysql($nome)` cria `MysqlDatabase`. `Database::createSqlite($nome)` cria `SqliteDatabase`. A fábrica não conecta nem executa queries por você.

Os drivers usam a trait `CoreDatabase` para estado da conexão, parâmetros, fila de consultas, resultados, erros e transações. O código específico de cada driver monta a configuração e o DSN de PDO.

## Ciclo normal

```php
$db = Database::createSqlite();
$db->setLocation(':memory:');
$db->connect();
$db->query('SELECT 1 AS numero')->submit();
$result = $db->getResult();
$db->disconnect();
```

`query()` prepara uma operação na fila. `submit()` é que percorre e executa essa fila. `getResult()` consulta o resultado já produzido; ele não aciona automaticamente uma query pendente.

## Composição de responsabilidades

| Operação | Onde ocorre |
| --- | --- |
| Escolher MySQL ou SQLite | Fábrica Database. |
| Ler perfil nomeado | `resolveDatabaseConfig()` na trait. |
| Montar DSN e criar PDO | `connect()` do driver e `establishConnection()`. |
| Preparar e executar SQL | Closure criada por `query()`, executada em `submit()`. |
| Vincular valores | `bind()` armazena; a closure consulta na execução. |
| Traduzir falha PDO | `filterPdoExceptionMessage()`, `onError()` e exceção do framework. |
| Agrupar operações atômicas | Métodos de transação; o chamador controla o escopo. |

## Estado e limites

Cada objeto possui sua própria conexão. Chamar `connect()` novamente num objeto já conectado lança exceção; não troca silenciosamente a conexão ativa. Para mudar de banco, desconecte deliberadamente ou crie outro objeto.

A fila de queries e os binds pertencem ao objeto. Não suponha que escrever várias chamadas fluentes crie automaticamente cópias independentes desses parâmetros.

O módulo não é ORM, migration runner nem query builder de tabelas. O SQL e o modelo de domínio continuam sendo responsabilidade da aplicação ou de bibliotecas que ela decidir incorporar.

## Como estudar com segurança

Comece com SQLite em memória: o banco desaparece ao encerrar a conexão, e não exige credenciais reais. Depois passe a um arquivo local descartável ou a um perfil MySQL exclusivo de desenvolvimento.

Leia o resultado completo para conhecer `data`, `found`, `affectedRows`, `lastId` e `label`. Uma query executada com sucesso pode não encontrar linha alguma; isso não equivale a uma falha de PDO.

## Onde conferir no código

- [`galastri/modules/Database.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Database.php)
- [`galastri/modules/database/traits/CoreDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/traits/CoreDatabase.php)
- [`galastri/modules/database/MysqlDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/MysqlDatabase.php)
- [`galastri/modules/database/SqliteDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/SqliteDatabase.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
