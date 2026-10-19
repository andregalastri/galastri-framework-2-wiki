> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Rotas — nós, endpoints e parâmetros

## A árvore é um array PHP

`app/config/routes.php` retorna um array. A primeira chave externa é a raiz da aplicação na URL. Dentro dela, chaves iniciadas por `/` representam nós; chaves iniciadas por `@` representam endpoints; as demais chaves reconhecidas representam configurações.

Este exemplo é um laboratório para sua cópia local:

```php
<?php
return [
    '/' => [
        'output' => 'json',
        'includeProjectMetadata' => false,
        '@main' => [],
        '/clientes' => [
            '@main' => [],
            '@detalhes' => [
                'parameters' => '/id/?aba',
            ],
        ],
    ],
];
```

| URL | Classe convencional | Método | Parâmetros |
| --- | --- | --- | --- |
| `/` | `app\controllers\Index` | `main()` | Nenhum. |
| `/clientes` | `app\controllers\Clientes` | `main()` | Nenhum. |
| `/clientes/detalhes/42` | A mesma classe Clientes | `detalhes()` | `id = '42'`, `aba = null` no Router. |
| `/clientes/detalhes/42/historico` | A mesma classe | `detalhes()` | `id = '42'`, `aba = 'historico'`. |
| `/clientes/detalhes` | Rota inválida | Não deve chegar ao controller JSON | Falta o parâmetro obrigatório. |
| `/clientes/detalhes/42/historico/extra` | Rota inválida | Não deve chegar ao controller JSON | Sobra um segmento. |

No controller, o getter de parâmetro converte o resultado nulo em `false`. A distinção entre o Router e o wrapper está detalhada na referência de Controller.

## @main é o endpoint de referência

Para saídas que exigem controller, cada nó terminal precisa declarar `@main`. Isso não significa executar `main()` antes de todos os endpoints. Ele é o endpoint usado quando não há outro nome de endpoint consumido; outro endpoint chama seu próprio método.

Mesmo quando a URL escolhe `detalhes()`, a classe precisa implementar `main(): array` por causa do contrato abstrato da base Controller.

## Configurações acumuladas

O Router percorre os nós aplicando configurações reconhecidas. Uma configuração definida na raiz pode continuar valendo em nós descendentes até que outro valor a substitua. Depois, as configurações do endpoint selecionado também são aplicadas.

Por exemplo, `output => 'json'` na raiz vale para `/clientes`, a menos que um nó ou endpoint altere o valor durante a resolução. Esse acúmulo acontece dentro da requisição; não significa que dois requests compartilhem a configuração de negócio.

## Nós dinâmicos

Uma chave como `/?cliente` captura o segmento correspondente em `getDynamicNode('cliente')`. O nome da classe é derivado do nome da chave, não do valor fornecido na URL.

Os nós fixos e endpoints reconhecidos têm precedência sobre a escolha dinâmica. Se houver vários nós dinâmicos no mesmo nível, o Router usa o primeiro compatível com esse percurso; não há um mecanismo de expressões de tipo por nó nessa resolução.

Leia os casos completos em [Nós dinâmicos e namespaces](/andregalastri/galastri-framework-2/wiki/Rotas-dinamicas-e-namespaces).

## Roteamento não valida o domínio

`/id` exige um segmento; não exige que ele seja um inteiro positivo nem que o registro exista. O controller deve verificar a entrada e a regra de negócio. Query string também não substitui um parâmetro obrigatório de caminho: `/clientes/detalhes?id=42` continua sem o segmento `/42`.

## Onde conferir no código

- [`app/config/routes.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/config/routes.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
