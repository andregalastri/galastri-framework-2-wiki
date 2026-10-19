> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# ListValidation — lista permitida com comparação estrita

## Regra disponível

`restrictList(int|string ...$valueList): self` registra a exigência de que o valor pertença à lista. No momento da verificação, usa `in_array(..., true)`.

```php
Validation::string()
    ->restrictList('rascunho', 'publicado', 'arquivado')
    ->onError('Situação inválida.')
    ->validate($situacao);
```

A regra não altera maiúsculas, espaços ou tipo do valor. `'Publicado'` não é a mesma string que `'publicado'`.

## Números e strings

Uma lista com `1` não admite automaticamente `'1'`. Isso importa ao ler valores de URL, que normalmente chegam como texto. Decida se o contrato é textual ou se haverá conversão e validação numérica antes da comparação.

## Limite da assinatura

O código chama `Tools::arrayFlatten()` internamente, mas a assinatura pública variádica aceita `int|string`, não array. Assim, `restrictList(['a', 'b'])` não é a forma de chamada admitida pela assinatura atual.

Para uma lista já preparada, use desempacotamento e confira seus tipos:

```php
$allowed = ['rascunho', 'publicado'];
Validation::string()->restrictList(...$allowed)->validate($situacao);
```

O fato de uma implementação interna saber achatar arrays não amplia automaticamente os tipos que PHP permite passar ao método.

## Casos de manutenção

Teste correspondência exata, string com caixa diferente, inteiro versus string numérica, valor ausente e lista vazia. A lista vazia não possui valor admitido e rejeita o que for verificado.

## Onde conferir no código

- [`galastri/modules/validation/traits/ListValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/ListValidation.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
