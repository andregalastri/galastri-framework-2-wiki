> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Controller — execução, retornos e acesso ao contexto

## Contrato de uma classe da aplicação

Um controller estende `galastri\modules\Controller` e implementa `protected function main(): array`. Seu construtor é final na base: não é um ponto de extensão para a aplicação.

```php
<?php
namespace app\controllers;

use galastri\modules\Controller;

final class Clientes extends Controller
{
    protected function main(): array
    {
        return ['message' => 'Lista de clientes'];
    }

    protected function detalhes(): array
    {
        return ['id' => $this->getParameter('id')];
    }
}
```

Crie-o em `app/controllers/Clientes.php` e configure os endpoints correspondentes no laboratório. A visibilidade `protected` permite a chamada pelo construtor herdado. Um método `private` na classe filha não oferece o mesmo acesso.

## O que o construtor final faz

Ele chama o método do endpoint que `Galastri` selecionou, chama o método HTTP adicional quando houver e reúne os retornos. As chamadas `doBefore` e `doAfter` aparecem comentadas no código; não são hooks ativos desta edição.

`main()` não é um preparador universal. Uma chamada ao endpoint `detalhes` executa `detalhes()`, não `main()` seguido de `detalhes()`. A obrigação de implementar `main()` é um contrato da classe abstrata.

## A composição do retorno

`mergeReturns()` usa `array_merge()` com o retorno do endpoint e o retorno HTTP, nessa ordem. Chaves textuais do segundo substituem as do primeiro. Arrays internos não são mesclados recursivamente; chaves numéricas seguem a regra de reindexação de `array_merge()`.

Se `includeProjectMetadata` estiver ativo, o núcleo acrescenta `meta` com `projectName`, `title`, `rootDir`, `baseHref`, `galastriVersion` e `appVersion`. Uma chave `meta` fornecida pelo controller é substituída por essa etapa.

O framework não acrescenta `error`, `warning`, `code`, `message` e `data` a todo sucesso automaticamente. A aplicação pode adotar esse envelope em seus retornos; o ErrorHandler monta sua própria estrutura nas falhas.

## Métodos auxiliares protegidos

| Grupo | Métodos | Observação |
| --- | --- | --- |
| Projeto e título | `setProjectName()`, `getProjectName()`, `setTitle()`, `getTitle()` | Leem ou atribuem valores em Config. |
| Saída e timezone | `getOutput()`, `getTimezone()` | Consultas; não há setter de output no controller. |
| View | `setTemplateFile()`, `getTemplateFile()`, `setViewFolder()`, `getViewFolder()` | Ajustam caminhos aceitos pelas definições. |
| Arquivo | `setFileFolder()`, `getFileFolder()`, `setFileContents()` | Escolhem pasta ou fornecem bytes, nome e MIME. |
| Download | `setDownloadableFile()`, `isDownloadableFile()` | Setter e getter separados no commit de referência. |
| Rota | `getParameterValues()`, `getParameter()`, `getDynamicNodeValues()`, `getDynamicNode()` | Delegam ao Router, com uma diferença para parâmetros nulos. |

`getResponse()` e `getFileContents()` são públicos para que a camada de saída obtenha os dados prontos. Não substituem o retorno do método de endpoint.

## Ausência de parâmetros

`Router::getParameter()` retorna `null` quando não há valor. `Controller::getParameter()` usa `?? false`; dentro do controller, portanto, o valor ausente é `false`.

```php
$aba = $this->getParameter('aba');

if ($aba === false) {
    $aba = 'resumo';
}
```

`'0'` é uma string presente. Usar `if (!$aba)` para detectar ausência confundiria esse valor com `false`.

## Alterações e riscos de contrato

Um método retornar `null` em vez de array pode falhar ao alimentar as propriedades tipadas da base. Um `echo` não substitui um retorno e pode contaminar JSON. Mudar a composição dos retornos afeta tanto templates quanto clientes de API, portanto teste as duas saídas ao alterar `mergeReturns()`.

## Onde conferir no código

- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
