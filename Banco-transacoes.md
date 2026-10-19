> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Transações — preservar integridade e controlar o escopo

## A pergunta que define o escopo

Se duas gravações precisam existir juntas, o sucesso de uma sem a outra pode produzir dados inconsistentes. Uma transação permite que o chamador confirme o conjunto ou desfaça as operações suportadas pelo banco naquele escopo.

O módulo oferece `begin()`, `commit()` e `cancel()`. `submit()` executa queries; ele não confirma automaticamente uma transação aberta e não envolve toda fila numa transação por conta própria.

## Um padrão explícito

Depois de conectar e criar as tabelas de laboratório:

```php
$db->begin();

try {
    $db->query('INSERT INTO notas (titulo) VALUES (:titulo)')
        ->bind(':titulo', 'Primeira nota')
        ->submit();

    $db->clearBind();
    $db->query('INSERT INTO notas (titulo) VALUES (:titulo)')
        ->bind(':titulo', 'Segunda nota')
        ->submit();

    $db->commit();
} catch (\Throwable $e) {
    try {
        $db->cancel();
    } catch (\Throwable $rollbackError) {
        error_log($rollbackError->getMessage());
    }

    throw $e;
}
```

`begin()` está antes do `try`: se ele recusar uma transação já aberta por outra camada, esse bloco não desfaz indiscriminadamente o trabalho de quem a abriu. O tratamento de rollback preserva a falha original mesmo se o cancelamento também falhar.

O exemplo usa apenas DML. Alguns bancos e instruções DDL podem causar confirmação implícita; a semântica do banco precisa ser considerada antes de declarar um conjunto inteiro como atômico.

## O que ocorre no segundo begin

Se PDO já estiver em transação, `begin()` lança `DATABASE_TRANSACTION_ALREADY_ACTIVE`. Ele não cria uma camada interna nem um savepoint.

Se a aplicação capturar essa exceção e continuar, a primeira transação continua sob controle do chamador. O módulo não marca automaticamente o estado para impedir um commit posterior. Portanto, rejeitar um segundo `begin()` e implementar transações aninhadas são soluções distintas; apenas a rejeição está presente neste commit.

## Quem é dono da transação

Evite que um serviço auxiliar chame `begin()` sem saber se o chamador já controla uma transação. Defina uma fronteira: por exemplo, o caso de uso abre e encerra a transação, e as rotinas internas fazem somente as queries.

Como o módulo não oferece um contrato público de nesting, inventar um contador por conveniência numa aplicação não cria automaticamente rollback parcial correto.

## Exercícios

1. Faça duas inserções válidas e confira ambas após commit.
2. Provoque uma violação de restrição na segunda e confira que cancelamento remove a primeira.
3. Abra uma transação, chame `begin()` novamente e capture a exceção; observe que isso não cria uma segunda camada.
4. Depois de falhar um submit, faça outra consulta e confira que a fila anterior não é executada novamente.

Use um banco SQLite em memória ou um banco descartável. O comportamento dos comandos SQL e da engine de armazenamento também participa da garantia final.

## Onde conferir no código

- [`galastri/modules/database/traits/CoreDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/traits/CoreDatabase.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
