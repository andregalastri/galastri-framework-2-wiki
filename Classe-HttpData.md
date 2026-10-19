> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# HttpData — leitura de query string e corpo da requisição

## A decisão de leitura

`HttpData` oferece uma visão combinada da query string e do corpo aceito pelo módulo. Ela não é simplesmente um apelido para `$_POST`.

1. Se `$_POST` não estiver vazio, retorna `array_replace($_GET, $_POST)`.
2. Caso contrário, lê `php://input`.
3. Reconhece `application/json` e tipos `application/...+json`, desconsiderando parâmetros depois de `;`.
4. Para JSON declarado com corpo não vazio, decodifica com exceção e exige objeto ou lista, representados como array PHP.
5. Em outros casos, tenta decodificar JSON sem a mesma obrigatoriedade; se não produzir array, conserva somente `$_GET`.

## Precedência

O corpo aceito substitui chaves de mesmo nome da query string. Não é uma mesclagem recursiva. Se a query traz `cliente => [...]` e o corpo também traz `cliente`, o valor do corpo substitui aquela chave inteira.

`array_replace()` também possui comportamento próprio para índices numéricos; receber uma lista JSON não significa concatená-la automaticamente a outros dados.

## API

| Método | Resultado |
| --- | --- |
| `getAll(): array` | Todos os dados combinados. |
| `getValue(string $key): mixed` | Valor da chave, ou nulo. |
| `has(string $key): bool` | Usa `array_key_exists()`; distingue presença de valor nulo. |
| `getKeys(): array` | Chaves do array combinado. |

Se um campo existe com `null`, `getValue()` retorna nulo e `has()` retorna verdadeiro. Essa diferença importa para atualizações parciais: “não enviado” pode ter significado diferente de “enviado para limpar”.

## JSON inválido

Um JSON declarado e malformado define HTTP 400 e lança exceção. Um JSON escalar, como `"texto"`, `123` ou `null`, também é rejeitado nesse caminho porque não resulta em array. Um objeto e uma lista são admitidos.

Números inteiros JSON grandes são preservados como strings por `JSON_BIGINT_AS_STRING`. Não force uma conversão para inteiro antes de avaliar se a precisão é necessária.

Um corpo vazio com Content-Type JSON não passa pelo mesmo bloco de rejeição de corpo não vazio. O resultado pode continuar sendo apenas a query string.

## Exemplo de uso no controller

```php
$data = HttpData::getAll();

if (!HttpData::has('nome')) {
    http_response_code(422);
    throw new \galastri\extensions\Exception('O campo nome é obrigatório.');
}

$nome = $data['nome'];
```

O trecho assume `use galastri\modules\HttpData;` no arquivo e pertence a um método do controller. Depois da leitura, ainda é necessário validar tipo, tamanho e regras de negócio.

## Limites de formato

Formulários POST preenchidos por PHP são atendidos pelo primeiro caminho. Um corpo URL-encoded enviado por PUT ou PATCH não é transformado automaticamente com `parse_str()` por este módulo. Uploads em `$_FILES` também não são uma API de upload de HttpData.

A aceitação de JSON sem Content-Type apropriado no ramo de tentativa não deve ser confundida com uma política rigorosa de formato. Um endpoint que exige JSON pode conferir explicitamente seu contrato HTTP.

O módulo não implementa autenticação, CSRF, autorização de campos ou validação de domínio. Ele resolve a leitura; o caso de uso decide o que pode fazer com os valores.

## Onde conferir no código

- [`galastri/modules/HttpData.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/HttpData.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
