> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Laboratório View — controller, template e conteúdo

## Objetivo

Criar uma página pequena para observar como os dados do controller chegam a `$g`. Faça o exercício em uma cópia local. Os caminhos apresentados são arquivos completos quando assim identificados.

## 1. Configurar a rota

Em `app/config/routes.php`:

```php
<?php
return [
    '/' => [
        'displayErrors' => true,
        'output' => 'view',
        'title' => 'Laboratório View',
        '@main' => [],
        '/laboratorio' => [
            '@main' => [],
        ],
    ],
];
```

O controller inicial continua necessário se você acessar `/`. O exercício abaixo usa `/laboratorio`.

## 2. Criar o controller

Arquivo `app/controllers/Laboratorio.php`:

```php
<?php
namespace app\controllers;

use galastri\modules\Controller;

final class Laboratorio extends Controller
{
    protected function main(): array
    {
        return [
            'nome' => 'Ana <colaboradora>',
            'itens' => ['Rotas', 'Respostas', 'Validação'],
        ];
    }
}
```

O array continua sendo dado. Não gere o HTML completo dentro dele para este exercício.

## 3. Preparar o template

Arquivo `app/templates/template.php`, na cópia de laboratório:

```php
<!doctype html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title><?php $g->print('meta', 'title'); ?></title>
    <base href="<?php $g->print('meta', 'baseHref'); ?>">
</head>
<body>
    <main>
        <?php $g->include('view'); ?>
    </main>
</body>
</html>
```

`$g` é criado por View antes de o template ser incluído. O template não precisa instanciá-lo.

## 4. Criar a view

Arquivo `app/views/Laboratorio/main.php`:

```php
<h1><?php $g->print('meta', 'title'); ?></h1>
<p>Olá, <?php $g->print('nome'); ?>.</p>
<ul>
<?php foreach ($g->get('itens') as $item) { ?>
    <li><?php echo htmlspecialchars($item, ENT_QUOTES, 'UTF-8'); ?></li>
<?php } ?>
</ul>
```

`get()` devolve o array para o laço. Como o item é impresso diretamente por PHP, ele recebe `htmlspecialchars()`. Para imprimir uma chave escalar do retorno, `print()` já aplica o escape HTML.

## 5. Conferir

Acesse `/laboratorio` no servidor local. O texto `Ana <colaboradora>` deve aparecer como texto visível, sem criar uma tag HTML. Confira também o código-fonte da resposta: os sinais devem estar escapados.

Troque temporariamente o caminho da view por um arquivo inexistente e observe o ErrorHandler. Depois restaure o arquivo. Esse caso ajuda a distinguir falha de resolução de apresentação de um erro na produção dos dados.

## O que aprender com o exercício

O caminho convencional é composto pelo namespace do controller sem `app\controllers`, seguido do método PHP e `.php`. Alterar `method` pode, portanto, alterar o nome de view procurado. Um `viewFile` explícito oferece outra forma de selecionar o arquivo.

O escape HTML não equivale a uma codificação universal: inserir dados em JavaScript, CSS ou uma URL exige o tratamento do contexto correspondente. Mantenha valores de configuração de include e caminhos sob controle da aplicação.

## Onde conferir no código

- [`galastri/core/output/View.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/View.php)
- [`galastri/core/output/engines/PhpEngine.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/engines/PhpEngine.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
