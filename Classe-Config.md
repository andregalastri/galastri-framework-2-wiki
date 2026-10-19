> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Config — registro e acesso às configurações

## Uma configuração é um objeto Definition

`Config` não armazena somente um array de valores soltos. Cada nome aponta para um objeto `Definition`, que conhece seu valor atual, tipos permitidos e proibidos, listas de valores, expressões regulares e contexto.

`Config::set('title', 'Clientes')` encontra a definição e chama `setValue()`. A atribuição passa pelo contrato da configuração. `Config::get('title')` consulta o valor, sem executar novamente o roteamento.

## Carregamento

`run()` reúne os arquivos `galastri/core/config/definitions/*.php` e `app/config/definitions/*.php`. Cada arquivo deve retornar um array de nomes e definições completas.

As definições internas são processadas antes das definições adicionais da aplicação. Um nome já registrado causa exceção; o diretório de aplicação serve para acrescentar definições, não para sobrescrever um contrato interno com o mesmo nome.

## API pública

| Método | Retorno ou efeito |
| --- | --- |
| `run(): void` | Importa e registra as definições. Não é um reset para várias requisições. |
| `set(string $name, mixed $value): void` | Exige que o nome exista e valida o novo valor. |
| `get(...$parameters): mixed` | Recebe o nome e, opcionalmente, um fallback. |
| `list(): array` | Retorna os objetos Definition, não apenas os valores. |
| `listValues(): array` | Extrai um array de nome para valor atual. |
| `exists($name, $throwException = false): bool` | Confere existência; pode lançar se estiver ausente. |
| `notExists($name, $throwException = false): bool` | Confere ausência; pode lançar se estiver presente. |
| `importConfig(string $path): array` | Verifica a presença do arquivo e executa `require`. |

## Fallback não é um novo registro

```php
$enabled = Config::get('configuracaoOpcional', false);
```

Se o acesso produzir a exceção tratada por `get()`, o segundo argumento será devolvido. O nome não passa a existir. O fallback também não valida nem corrige um valor que já esteja registrado.

O próprio ErrorHandler usa fallbacks para conseguir responder quando uma falha ocorre antes de a configuração estar pronta. Sem esse recurso, tratar uma configuração ausente poderia provocar outra falha na apresentação do erro.

## Teste isolado em teste.php

```php
<?php
require __DIR__.'/galastri/bootstrap/constants.php';
require __DIR__.'/vendor/autoload.php';

use galastri\core\config\Config;
use galastri\extensions\Exception;

Config::run();
Config::set('title', 'Laboratório');

echo Config::get('title').PHP_EOL;
echo Config::get('ausente', 'fallback').PHP_EOL;

try {
    Config::set('output', 'xml');
    throw new \RuntimeException('O valor inválido foi aceito.');
} catch (Exception $e) {
    echo $e->getCode().PHP_EOL;
}
```

Resultados esperados: `Laboratório`, `fallback` e o código `CONFIG_INVALID_VALUE`. O teste não inclui o bootstrap completo para não atender a aplicação junto da verificação.

## Atenção para referências e consumo

`list()` devolve um array com objetos. Copiar o array não clona cada Definition; ainda é possível observar ou modificar os mesmos objetos. O contrato público usual de atribuição continua sendo `set()`.

Uma configuração existir também não prova que tenha consumidor. As flags `useErrorHandler` e `performanceAnalysis` são exemplos que precisam ser lidos junto do fluxo atual, como explica a referência de debug.

## Onde conferir no código

- [`galastri/core/config/Config.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Config.php)
- [`galastri/core/config/Definition.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Definition.php)
- [`galastri/core/ErrorHandler.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
