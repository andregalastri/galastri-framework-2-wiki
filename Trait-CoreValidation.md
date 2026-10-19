> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# CoreValidation — execução inversa da cadeia

## Montagem e execução são momentos separados

Os métodos de regra acrescentam closures a `validationChain`. `onError()` também acrescenta uma closure, cuja função é configurar mensagem, código e dados adicionais. Ao chamar `validate($value)`, o objeto armazena o valor e percorre `array_reverse($validationChain)`.

## Uma cadeia vista de trás para frente

```php
$validator->minLength(3)
    ->onError('Texto muito curto.')
    ->maxLength(10)
    ->onError('Texto muito longo.');
```

A execução é:

| Ordem real | Ação |
| --- | --- |
| 1 | Seleciona a mensagem “Texto muito longo.” |
| 2 | Verifica `maxLength(10)`. |
| 3 | Seleciona a mensagem “Texto muito curto.” |
| 4 | Verifica `minLength(3)`. |

A primeira regra escrita não é necessariamente a primeira regra executada. Isso afeta qual falha aparece se um valor não atende a várias regras, e também quando uma regra pressupõe que outra já verificou o tipo ou a ausência.

## Não depender de uma verificação anterior imaginária

Se uma regra chama `mb_strlen()` e outra rejeita um valor ausente, posicioná-las visualmente na ordem desejada não basta. Verifique a ordem invertida que de fato será executada. Para um valor de tipo inadequado, a falha pode ser um erro PHP da operação antes de chegar à mensagem pretendida.

Um teste útil fornece `null`, array, string vazia e um valor válido, além do caso comum de tamanho incorreto.

## throwError

A rotina ativa o sinal estático de exibição de mensagem da Validation e lança a Exception do framework. Se o template tiver placeholders, usa `messageFlagValues`; caso contrário, envia a mensagem e dados adicionais diretamente.

As regras especializadas preenchem `messageFlagValues` conforme sua falha. Uma mensagem personalizada com uma quantidade arbitrária de placeholders pode não corresponder aos valores que a regra oferece. Prefira uma mensagem literal de negócio quando não precisar desses detalhes técnicos.

## Persistência e retorno

`validate()` retorna `void`; uma cadeia vazia é aceita sem ação. A implementação não limpa as regras depois do sucesso ou falha. Essa persistência pode ser útil para aplicar o mesmo contrato a vários valores, mas exige objetos distintos para contratos distintos.

O construtor prepara a mensagem padrão por `Message::get('VALIDATION_FAIL')`. Ao mudar a execução da cadeia, teste mensagens por regra e o consumo do sinal `displayError`, além da rejeição do dado.

## Onde conferir no código

- [`galastri/modules/validation/traits/CoreValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/CoreValidation.php)
- [`galastri/modules/Validation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Validation.php)
- [`galastri/extensions/Exception.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/extensions/Exception.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
