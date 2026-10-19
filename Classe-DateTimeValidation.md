> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# DateTimeValidation — parsing estrito e comparação de datas

## Regras

`minDateTime()`, `maxDateTime()` e `dateTimeRange()` registram limites. `validate($value, $format = 'Y-m-d H:i:s')` escolhe o formato usado para interpretar o valor testado e delega à cadeia comum.

Os limites podem ser `\DateTime` ou string. O tipo declarado não inclui `DateTimeImmutable`; converter ou ampliar esse contrato exigiria uma decisão explícita de implementação.

## Exemplo de intervalo

```php
Validation::dateTime()
    ->dateTimeRange('2026-01-01', '2026-12-31', 'Y-m-d')
    ->onError('A data deve estar dentro do ano de 2026.')
    ->validate($data, 'Y-m-d');
```

O terceiro argumento de `dateTimeRange()` define o formato dos limites textuais. O segundo argumento de `validate()` define o formato do valor testado. Quando o formato do limite não é informado, a implementação usa o formato do valor.

Os limites são inclusivos. A comparação é entre objetos de data/hora produzidos pelo parsing ou fornecidos pelo chamador.

## Como uma string é interpretada

A rotina chama `DateTime::createFromFormat('!'.$format, $value)` e consulta `DateTime::getLastErrors()`. O prefixo `!` zera componentes não fornecidos, evitando que dependam acidentalmente da hora atual.

Somente um objeto sem avisos ou erros de parsing é aceito. Isso impede tratar uma data como `2026-02-31` como um dia ajustado silenciosamente para outro mês. `ValueError`, como em determinadas entradas inválidas, também é tratado no caminho de parsing.

Se o parsing falhar, o código troca a mensagem para `VALIDATION_INVALID_DATETIME` e lança. Essa mensagem de formato pode prevalecer sobre a mensagem de intervalo configurada, pois o problema ocorreu antes da comparação de limites.

## Timezone e objetos fornecidos

Objetos `DateTime` fornecidos passam diretamente pela rotina; strings são interpretadas com as regras e timezone disponíveis no processo. A configuração `timezone` do framework é aplicada por Galastri antes do controller.

Uma data de calendário sem horário e um instante com fuso não são o mesmo contrato. Escolha o formato e os limites conforme o domínio e teste mudanças de dia e horário quando relevantes.

## Cadeia vazia não valida formato

Chamar somente `Validation::dateTime()->validate('texto inválido')` não aciona uma regra de parsing, pois a cadeia pode estar vazia. O nome da fábrica não acrescenta uma regra invisível.

Para o exercício, use um limite ou intervalo real do domínio. Não invente um método `isDateTime()` que não existe na classe.

## Casos para uma contribuição

Inclua data válida, mês inexistente, dia inexistente, ano bissexto, horário fora do formato, byte nulo, limite inferior e superior e um objeto `DateTime`. Um exemplo válido isolado não verifica a rejeição de normalizações automáticas do parser.

## Onde conferir no código

- [`galastri/modules/validation/DateTimeValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/DateTimeValidation.php)
- [`galastri/modules/validation/traits/CoreValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/CoreValidation.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
