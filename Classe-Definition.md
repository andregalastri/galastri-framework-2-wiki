> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Definition — como um valor é aceito ou rejeitado

## O contrato nasce antes do valor

O construtor recebe nome e propriedades da definição, prepara tipos, listas e regex, valida o contexto e por último chama `setValue()` com `defaultValue`. Isso significa que o valor padrão também precisa obedecer ao contrato.

| Propriedade obrigatória | Tipo da propriedade | Significado |
| --- | --- | --- |
| `defaultValue` | Qualquer valor aceito pela definição | Valor inicial, submetido à validação. |
| `validTypes` | Array de strings | Tipos admitidos; vazio não restringe por esta lista. |
| `invalidTypes` | Array de strings | Tipos proibidos. |
| `validValues` | Array | Lista estrita de valores admitidos. |
| `invalidValues` | Array | Lista estrita de valores proibidos. |
| `validRegex` | String | Expressão que deve corresponder quando aplicável. |
| `invalidRegex` | String | Expressão que não pode corresponder. |
| `context` | String | `debug`, `route`, `node` ou `endpoint`. |

`Config::run()` exige a presença dessas chaves. Informar somente `defaultValue` e `validTypes` não cria uma definição válida.

## Ordem de setValue

O método verifica tipos permitidos, tipos proibidos, valores permitidos, valores proibidos, regex proibida e regex obrigatória. Somente depois de passar pelas verificações a propriedade de valor é atribuída.

As listas de valores usam comparação estrita. Assim, `1`, `'1'` e `true` não são intercambiáveis. A identificação de tipos usa `Tools::typeOf()`: escreva `bool`, `int`, `float`, `string`, `array` etc., conforme esse vocabulário.

## A exceção da string vazia

As expressões regulares são verificadas dentro de `if ($value !== '')`. A string vazia pode, portanto, ser um padrão permitido mesmo quando uma regex aceita somente caminhos não vazios. Isso explica contratos como `viewFile`: `''` significa resolver pelo caminho convencional.

Não conclua que uma regex é a única regra de uma configuração. Tipos e listas são aplicados antes, e a string vazia recebe esse tratamento particular.

## Contexto é metadado validado

`getContext()` devolve o contexto declarado. Ele não percorre a árvore de rotas nem decide sozinho em qual nível uma chave pode aparecer. O Router possui pontos explícitos de leitura para cada configuração.

Uma nova definição de aplicação com contexto `route` continua precisando de um consumidor. Se a intenção é atribuir seu valor por `routes.php`, será necessário estudar a integração de leitura no Router; só acrescentar o arquivo de definição não realiza essa parte.

## Dados adicionais de falha

Quando um valor é rejeitado, `setValue()` inclui `testedValue` nos dados da exceção. O Router usa isso para produzir mensagens mais específicas sobre caminhos e namespaces. Ao alterar esse nome ou remover o dado, confira os `catch` que chamam `getData('testedValue')`.

## Acesso público

`getName()`, `getValue()`, `getValidTypes()`, `getInvalidTypes()`, `getValidValues()`, `getInvalidValues()`, `getValidRegex()`, `getInvalidRegex()` e `getContext()` são consultas. `setValue()` faz a atribuição validada. As propriedades que formam o contrato são preparadas por métodos privados do construtor.

## Exercício de manutenção

Imagine restringir `title` a um comprimento máximo. Uma Definition não possui hoje uma propriedade `maxLength`. Acrescentá-la exigiria definir o novo contrato, integrar sua leitura, aplicar a validação e revisar todas as definições existentes se ela se tornasse obrigatória. A documentação não deve ensinar esse atributo como se já estivesse disponível.

## Onde conferir no código

- [`galastri/core/config/Definition.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Definition.php)
- [`galastri/core/config/Config.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Config.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
