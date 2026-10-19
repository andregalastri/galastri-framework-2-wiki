> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# StringValidation — tamanho, conjuntos e formatos

## Regras disponíveis

Além das regras compartilhadas de ausência e listas, esta classe oferece tamanho, conjuntos de caracteres, palavras permitidas ou proibidas, quantidade mínima de grupos, caixa de letras e formatos específicos.

| Métodos | Verificação |
| --- | --- |
| `minLength($n)`, `maxLength($n)`, `lengthRange($min, $max)` | Comprimento por `mb_strlen()`. |
| `allowChars(...$chars)` | Rejeita caracteres fora do conjunto. |
| `denyChars(...$chars)` | Rejeita qualquer ocorrência do conjunto. |
| `allowWords(...$words)` | Exige correspondência integral com uma alternativa. |
| `denyWords(...$words)` | Rejeita ocorrências das alternativas como substring. |
| `requiredChars(...$groups)` | Exige uma quantidade mínima de ocorrências por grupo. |
| `denyUpperCase()`, `denyLowerCase()` | Verificam caixa entre os caracteres classificados como letras. |
| `isCpf()`, `isCnpj()` | Formato numérico e dígitos verificadores implementados. |
| `isEmail()` | Usa `FILTER_VALIDATE_EMAIL`. |
| `isPhone()` | Usa os padrões brasileiros definidos no arquivo. |

## Literal não é uma faixa regex

`allowChars('a-z')` informa os caracteres literais `a`, `-` e `z`, porque o código escapa conjuntos não reconhecidos com `preg_quote()`. Para uma categoria pronta, use uma flag como `--letters`.

```php
Validation::string()
    ->allowChars('--lettersUtf8', '--spaces', '-')
    ->onError('Use letras, espaços ou hífen.')
    ->validate($nome);
```

Nesse conjunto, hífen é literal. As flags são fragmentos internos previstos pela classe; não são comandos de terminal nem parâmetros de rota.

## Grupos e ocorrências mínimas

```php
Validation::string()
    ->requiredChars(['--upperLetters', 1], ['--numbers', 2])
    ->onError('Informe ao menos uma maiúscula e dois números.')
    ->validate($codigo);
```

A regra conta ocorrências do grupo, não necessariamente caracteres distintos. Para um só grupo, também aceita `requiredChars('--numbers', 2)`. A quantidade mínima deve ser escolhida de forma significativa pela aplicação; a implementação não estabelece uma política completa de senha.

O texto literal de um grupo continua sendo escapado. `requiredChars('a-z', 2)` não pede duas letras quaisquer entre a e z.

## Palavras: duas semânticas diferentes

`allowWords('sim', 'não')` aceita somente uma das strings inteiras, com comparação sensível à caixa e sem trim automático. `denyWords('admin')` encontra a substring, inclusive dentro de outra palavra.

Não use flags de categorias como se `allowWords('--letters')` significasse aceitar qualquer palavra feita de letras. As flags são fragmentos regex reutilizados pela implementação, e o modo de palavras não os envolve automaticamente como uma classe de caracteres com repetição.

## Caixa e caracteres não alfabéticos

As regras de caixa filtram as letras Unicode e verificam se elas pertencem à categoria exigida. Números e pontuação não obrigam a presença de letra. Por exemplo, uma string sem letras pode passar por uma regra de proibição de maiúsculas; use outra regra se letras forem obrigatórias.

## Formatos específicos

CPF aceita 11 dígitos ou a forma pontuada prevista, rejeita sequências repetidas e confere verificadores. CNPJ aceita 14 dígitos ou a máscara numérica prevista e também confere verificadores. Qualquer outro formato, inclusive alfanumérico, não é atendido por essa implementação.

`isEmail()` não consulta DNS nem comprova posse da caixa postal. `isPhone()` não comprova que o número existe, está ativo ou pertence ao usuário. São validações do formato implementado.

## Tipos e limites

Nem toda regra começa com uma checagem `is_string()`. Fornecer um array a uma regra que chama `mb_strlen()` pode produzir erro de tipo, em vez da mensagem de negócio desejada. Confira o tipo na fronteira da aplicação e teste entradas inesperadas.

A flag `--numbersUtf8` usa `\p{Nl}` no código, que é uma categoria Unicode específica; não significa “todos os dígitos de todos os alfabetos”. As expansões completas estão na referência da API.

## Casos para testar

Inclua acentos, emoji, hífen literal, alternativas de palavra, substring proibida e combinação de regras com mensagens próprias. Em comprimento, `mb_strlen()` não equivale necessariamente ao número de símbolos que uma pessoa percebe quando há sequências Unicode combinadas.

## Onde conferir no código

- [`galastri/modules/validation/StringValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/StringValidation.php)
- [`galastri/modules/validation/traits/CoreValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/CoreValidation.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
