> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Extensões — adaptação de classes da linguagem

## O significado do diretório

`galastri/extensions/` contém adaptações de classes da linguagem usadas pelo framework. No commit de referência, a peça presente é `galastri\extensions\Exception`, que estende `\Exception`.

Esse diretório não é o local padrão para instalar pacotes de terceiros. Dependências externas são declaradas e carregadas pelo Composer. Um nome de pasta “extensions” também não significa que exista um gerenciador dinâmico de plugins.

## Por que a exceção é uma fronteira importante

Ela traduz o catálogo `Message`, aplica placeholders, guarda dados adicionais e recebe o sinal de exibição de mensagem da validação. Muitos módulos usam essa classe ao rejeitar entradas ou configurações.

Uma mudança no construtor pode, portanto, afetar Config, Router, validação, banco e o próprio caminho de tratamento de erros. Testar somente `throw new Exception('texto')` não verifica todas as formas de chamada que o framework utiliza.

## Orientação para novas adaptações

Antes de criar outra classe aqui, explique qual contrato da classe original está sendo acrescentado e quais consumidores dependem disso. Uma conveniência específica de uma aplicação pode pertencer a `app/`, enquanto uma ferramenta independente pode pertencer a `modules/`.

A organização registrada pelo autor favorece composição; a herança da exceção é um uso concreto existente. Documente cada caso pelo que ele faz, sem transformar a exceção ao padrão numa proibição ou permissão irrestrita de herança.

## Onde conferir no código

- [`galastri/extensions/Exception.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/extensions/Exception.php)
- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
