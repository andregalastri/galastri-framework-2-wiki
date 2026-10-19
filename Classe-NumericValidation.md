> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# NumericValidation — números, comparações e conversões

## Regras de faixa

`minValue()`, `maxValue()` e `valueRange()` verificam se o dado é inteiro, float ou string, se é numérico e se sua conversão para float é finita. Depois aplicam os limites, que são inclusivos.

```php
Validation::number()
    ->valueRange(1, 100)
    ->onError('Informe uma quantidade entre 1 e 100.')
    ->validate($quantidade);
```

Uma string numérica pode passar. Booleanos, arrays, objetos, `INF` e `NAN` não são valores válidos para essas regras de faixa.

## As proibições não são equivalentes a uma faixa

| Regra | O que faz |
| --- | --- |
| `denyFloat()` | Rejeita um valor cujo tipo PHP já é float. |
| `denyZero()` | Converte para float e rejeita se o resultado for exatamente `0.0`. |
| `denyNegative()` | Converte para float e rejeita se for menor que zero. |

`denyFloat()` sozinho não significa “apenas inteiros”: a string `'1.5'` não é do tipo float e pode passar. `denyNegative()` sozinho também não é uma validação numérica completa: determinadas entradas não numéricas podem converter para zero.

Use uma regra com verificação de número finito quando esse for o contrato. Se a aplicação precisa de um inteiro textual estrito, defina essa exigência explicitamente, sem supor que a combinação dos nomes dos métodos a implemente.

## O validador não converte o retorno

`validate()` retorna `void`. Mesmo quando uma comparação converte internamente o dado, a variável do controller não é substituída por um inteiro normalizado. A aplicação decide a representação apropriada depois de validar.

Para valores com precisão decimal relevante, uma conversão para float pode perder informação. A presença de `is_finite()` evita infinito, mas não fornece aritmética decimal exata nem um contrato específico de dinheiro.

## Ordem da cadeia

A execução é inversa, como nos demais validadores. Ao combinar uma regra de faixa com proibições, confira qual roda primeiro e qual mensagem será selecionada. Uma regra que faz cast pode agir antes da que detectaria um tipo inválido.

## Exercícios

Compare `1`, `1.0`, `'1'`, `'1.5'`, `0`, `'0'`, `-0.1`, `'abc'`, `true`, `INF` e `NAN`. Faça isso para cada regra isolada antes de interpretar uma cadeia inteira.

Uma correção em comparação numérica precisa demonstrar tanto o caso rejeitado quanto os zeros e números válidos que devem continuar aceitos.

## Onde conferir no código

- [`galastri/modules/validation/NumericValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/NumericValidation.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
