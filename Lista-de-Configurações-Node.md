> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Configurações do contexto node

Os valores abaixo vêm das definições desta edição. O arquivo de rotas da aplicação pode substituir parte deles durante o percurso.

| Nome | Tipo admitido | Valor padrão | Efeito ou limite |
| --- | --- | --- | --- |
| `controller` | `string` | `""` | Substitui o nome terminal do controller do nó selecionado. |

## O momento da aplicação

Depois de resolver os nós, `configureNodeProperties()` verifica `controller`. Quando informado, ele substitui o último segmento do namespace já composto. A configuração atua no nó terminal selecionado; não é uma lista de controllers que serão executados sucessivamente.

## Exemplo

```php
'/clientes' => [
    'controller' => 'CadastroClientes',
    '@main' => [],
],
```

Este trecho vai dentro da raiz de `routes.php`. Com o namespace padrão, ele pretende atender `app\controllers\CadastroClientes`, no arquivo correspondente.

O contrato da Definition exige string, mas não impõe por si toda a sintaxe de nome de classe. Uma string que passa pela Definition ainda pode levar a uma classe inexistente na etapa de Galastri.

## Exercício de manutenção

Mude somente o nome do controller neste nó e compare `Router::getControllerNamespace()` com a localização do arquivo. Não altere simultaneamente o endpoint, o namespace e a saída: isso dificulta atribuir a mudança de comportamento à causa correta.

## Onde conferir no código

- [`galastri/core/config/definitions/node.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/definitions/node.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
