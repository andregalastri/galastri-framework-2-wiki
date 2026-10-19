> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# EmptyValidation — nulo, vazio e zero

## denyNull

`denyNull()` rejeita somente `null`, usando comparação estrita. Uma string vazia, `false` ou um array vazio não são nulos e não são rejeitados por essa regra sozinha.

## denyEmpty

A regra copia o valor e, se ele for string, aplica `trim()` antes de comparar. Ela rejeita `null`, `false`, `''` e `[]`. Os valores `0`, `0.0` e `'0'` permanecem admitidos por esta verificação.

| Entrada | denyNull | denyEmpty |
| --- | --- | --- |
| `null` | Rejeita | Rejeita |
| `false` | Admite | Rejeita |
| `''` | Admite | Rejeita |
| `'   '` | Admite | Rejeita |
| `[]` | Admite | Rejeita |
| `0`, `0.0`, `'0'` | Admite | Admite |

“Admite” significa apenas que essa regra não lançou; não significa que o valor seja válido para todas as regras seguintes.

## O trim não corrige o dado

A regra usa uma cópia para testar ausência. Ela não devolve uma string normalizada nem substitui o valor original do controller. Se a aplicação quer remover espaços antes de gravar, isso é uma decisão explícita de normalização.

## Exemplo

```php
Validation::string()
    ->denyEmpty()
    ->onError('Informe o nome.')
    ->validate($nome);
```

`onError()` é executado primeiro na ordem inversa e configura a mensagem antes de a regra verificar o dado.

## Teste de regressão

Inclua zero numérico e string zero ao corrigir ausência de campos. Usar simplesmente `empty($value)` mudaria o comportamento desses valores. Uma alteração assim pode rejeitar quantidades, índices e opções que eram válidos.

## Onde conferir no código

- [`galastri/modules/validation/traits/EmptyValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/EmptyValidation.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
