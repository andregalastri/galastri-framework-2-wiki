> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Criar uma definição adicional de configuração

## Objetivo do laboratório

Criar `itemsPerPage` como configuração da aplicação para observar o registro e a validação. O exemplo não modifica o núcleo e não cria leitura automática dessa chave em `routes.php`.

## 1. Criar o arquivo

Na cópia local, crie `app/config/definitions/paginacao.php`:

```php
<?php
return [
    'itemsPerPage' => [
        'defaultValue' => 20,
        'validTypes' => ['int'],
        'invalidTypes' => [],
        'validValues' => [10, 20, 50],
        'invalidValues' => [],
        'validRegex' => '',
        'invalidRegex' => '',
        'context' => 'route',
    ],
];
```

O nome do arquivo organiza as definições; o nome público da configuração é a chave `itemsPerPage`. Uma pasta ausente deve ser criada. Não copie uma definição interna com o mesmo nome para tentar substituí-la.

## 2. Preparar teste.php

```php
<?php
require __DIR__.'/galastri/bootstrap/constants.php';
require __DIR__.'/vendor/autoload.php';

use galastri\core\config\Config;
use galastri\extensions\Exception;

Config::run();
echo Config::get('itemsPerPage').PHP_EOL;
Config::set('itemsPerPage', 50);
echo Config::get('itemsPerPage').PHP_EOL;

try {
    Config::set('itemsPerPage', 30);
    throw new \RuntimeException('A configuração aceitou um valor fora da lista.');
} catch (Exception $e) {
    echo $e->getCode().PHP_EOL;
}
```

Execute `php teste.php`. Os resultados esperados são `20`, `50` e `CONFIG_INVALID_VALUE`.

## 3. Usar depois que Config estiver carregado

Um controller pode consultar `Config::get('itemsPerPage')` e decidir como empregá-lo numa consulta. Ele também pode atribuir um valor válido com `Config::set()`.

Não coloque essa consulta no `app/bootstrap.php` como se as definições já existissem: esse arquivo roda antes de `Galastri::run()` carregar Config.

## 4. Se a contribuição for ao framework

Para uma configuração interna nova, além de registrar o contrato, localize o componente responsável por consumi-la. Se o valor tiver de ser herdado por rota, integre a leitura no Router. Se afetar a saída, revise como o controller e a classe de saída leem o estado.

Documente o padrão e a exceção. Teste ao menos ausência da chave na rota, atribuição válida e atribuição rejeitada. Um atributo novo que existe na tabela, mas nunca é lido pelo fluxo, cria uma expectativa falsa para quem configura o projeto.

## Onde conferir no código

- [`galastri/core/config/Config.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Config.php)
- [`galastri/core/config/Definition.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Definition.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
