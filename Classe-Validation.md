> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Validation — fábricas, regras e mensagens

## Instanciar um validador

`Validation::string()` devolve `StringValidation`, `number()` devolve `NumericValidation` e `dateTime()` devolve `DateTimeValidation`. As fábricas criam objetos novos.

As chamadas fluentes registram regras. A verificação do valor ocorre em `validate()`, que retorna `void` e lança a exceção do framework se uma regra falhar. O objeto não devolve automaticamente um valor corrigido.

## Um exemplo de regra e mensagem

```php
$validation = Validation::string();
$validation->minLength(3)->onError('Use ao menos 3 caracteres.');
$validation->maxLength(50)->onError('Use no máximo 50 caracteres.');
$validation->validate($nome);
```

O trecho assume a importação da fábrica. Cada `onError()` vem depois da regra a que queremos associar a mensagem nesse encadeamento. A ordem interna da execução é **inversa**; entenda-a na referência CoreValidation antes de montar cadeias complexas.

## O nome da fábrica não é uma verificação completa de tipo

`Validation::string()->validate($valor)` sem regras não faz uma validação geral de string. A cadeia vazia retorna sem verificações. O mesmo cuidado vale para número e data/hora.

Cada regra possui sua implementação. Algumas verificam tipo explicitamente; outras chamam funções de string ou convertem para float. Não atribua ao conjunto inteiro um contrato de rejeição uniforme que só alguns métodos implementam.

## Mensagens para pessoas

Ao falhar, o núcleo da validação ativa `Validation::$displayError`. A exceção personalizada captura esse sinal e o limpa, permitindo ao ErrorHandler mostrar a mensagem da validação sem liberar automaticamente arquivo, linha ou trace.

Uma falha de validação não escolhe por si HTTP 422. Se esse for o contrato da API, o fluxo da aplicação precisa definir o status apropriado antes de a falha ser apresentada.

## Regras de ausência e listas

String, número e data/hora compartilham `denyNull()`, `denyEmpty()` e `restrictList()`. `denyEmpty()` preserva zero numérico e a string `'0'`, enquanto rejeita, por exemplo, string somente com espaços. `restrictList()` usa comparação estrita.

## Reutilização

`validate()` muda o valor de trabalho e percorre as regras, mas não apaga a cadeia. Para campos com contratos diferentes, crie objetos separados. Ao reutilizar o mesmo validador para vários valores, mantenha em mente que regras e configuração de mensagens pertencem ao objeto.

## Sequência de estudo

Leia CoreValidation para a ordem; EmptyValidation e ListValidation para regras compartilhadas; depois as três classes especializadas para conhecer o que cada uma realmente testa.

## Onde conferir no código

- [`galastri/modules/Validation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Validation.php)
- [`galastri/modules/validation/traits/CoreValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/CoreValidation.php)
- [`galastri/extensions/Exception.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/extensions/Exception.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
