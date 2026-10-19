> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# CoreDatabase — filas, binds, resultados e falhas

## Estado do objeto

A trait reúne PDO, flag de conexão, binds, resultados com e sem rótulo, fila de consultas e fila de tratamento de erro. Os setters de host, porta, banco, usuário, senha, opções e localização retornam o próprio objeto.

`establishConnection()` recusa uma conexão já ativa, verifica as propriedades obrigatórias do driver e prepara PDO. `disconnect()` elimina a referência e limpa os estados de fila, parâmetros e resultados listados na implementação.

## Query é adiada

`query($sql, $label = null)` acrescenta uma closure a `queryChain`. Ela prepara a instrução, vincula os parâmetros existentes naquele momento de execução, executa PDO e processa o resultado. `submit()` percorre as closures e limpa as filas de query e erro em `finally`, inclusive se alguma operação falhar.

As consultas restantes não são executadas depois de uma exceção. Limpar a fila não desfaz uma gravação já realizada: para atomicidade, use uma transação apropriada.

## Binds pertencem ao objeto

```php
$db->query('SELECT :numero AS numero');
$db->bind(':numero', 10);
$db->bind(':numero', 20);
$db->submit();
```

A consulta vê o valor final armazenado antes da execução, não uma cópia capturada na chamada a `query()`. Pelo mesmo motivo, não encadeie duas queries com binds diferentes supondo snapshots independentes.

Use `query()->bind()->submit()` para uma operação, depois `clearBind()` antes de outra com parâmetros diferentes. O submit limpa filas, mas **não limpa automaticamente todos os binds**.

## Tipos de parâmetros

Strings usam `PDO::PARAM_STR`, inteiros `PARAM_INT`, floats `PARAM_STR`, booleanos `PARAM_BOOL`, nulos `PARAM_NULL` e recursos `PARAM_LOB`. O código possui ainda um ramo para determinados valores vazios de tipos fora desse mapa.

Arrays e objetos não são uma forma de passar listas SQL automaticamente. Para um `IN (...)`, construa uma quantidade controlada de placeholders e vincule os valores correspondentes. Placeholders também não substituem nomes de tabela, coluna ou palavras-chave; esses nomes exigem seleção controlada pela aplicação.

## Resultados

| Campo | Significado |
| --- | --- |
| `label` | Rótulo informado à query, ou nulo. |
| `data` | Linhas associativas quando a instrução retorna colunas. |
| `found` | Para resultados tabulares, indica se houve linhas; para outras instruções, se houve linhas afetadas. |
| `affectedRows` | Valor retornado pelo driver em `rowCount()`. |
| `lastId` | Último identificador obtido de PDO, com fallback do módulo. |

Para SELECT, `found` é derivado das linhas lidas. Não use `affectedRows` como contador portátil de SELECT entre drivers. `lastId` faz sentido no contexto da inserção correspondente; uma consulta posterior pode continuar refletindo o último ID da conexão.

`getResult()` lê o último resultado **sem rótulo**. `getResult('nome')` lê o resultado nomeado e lança se ele não existir. Uma query com rótulo não necessariamente atualiza o resultado sem rótulo. `getLastId()` e `hasResult()` aceitam a mesma escolha de rótulo.

Antes da execução, o espaço do resultado correspondente é limpo para reduzir o risco de retornar o valor antigo de uma consulta que acabou falhando.

## onError e falhas PDO

`onError($message, $code, $additionalData = [])` registra uma política de tradução. O código pode corresponder a SQLSTATE, código do driver ou texto identificado no detalhe da restrição, conforme a rotina de comparação.

A mensagem pode ser uma string ou um array com template e valores de substituição. Se não houver regra compatível, o módulo preserva uma mensagem técnica de PDO tratada para o mecanismo de placeholders.

Essas regras são estado do objeto e participam da execução da fila, não callbacks de sucesso de uma query. Uma mensagem de conflito deve representar a restrição efetivamente identificada; SQLSTATE amplo não garante que o conflito tenha sido de um campo específico.

## Transações e limpeza

`begin()`, `commit()` e `cancel()` delegam ao PDO depois de verificar a conexão. `begin()` recusa uma transação já ativa. `cancel()` limpa filas, binds e resultados após o rollback. `commit()` confirma, mas não é uma limpeza universal do objeto.

Não há suporte público a savepoints, transações aninhadas ou marcação automática de “somente rollback” nesta edição. O capítulo de transações mostra as implicações.

## Onde conferir no código

- [`galastri/modules/database/traits/CoreDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/traits/CoreDatabase.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
