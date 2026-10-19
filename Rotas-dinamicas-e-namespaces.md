> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Nós dinâmicos, nomes de classes e subdiretórios

## Uma captura dinâmica completa

Adicione ao array da raiz, em uma cópia de laboratório:

```php
'/?cliente' => [
    '@main' => [],
    '@pedidos' => [
        'parameters' => '/?pagina',
    ],
],
```

Esse trecho é **parte de `routes.php`**, não um arquivo de configuração completo. Na URL `/acme/pedidos/2`, a captura `cliente` recebe `acme`, o endpoint é `pedidos`, e `pagina` recebe `'2'`.

O controller convencional desse nó é `app\controllers\Cliente`, porque `/?cliente` é transformado para o nome de classe. Não será procurada uma classe `Acme`.

```php
protected function pedidos(): array
{
    return [
        'cliente' => $this->getDynamicNode('cliente'),
        'pagina' => $this->getParameter('pagina'),
    ];
}
```

Esse método pertence à classe `Cliente`, que também precisa implementar `main()`.

## Precedência

O Router tenta um nó fixo correspondente, verifica se o segmento já representa um endpoint do nível atual e somente então percorre as chaves dinâmicas. Assim, um nó `/login` pode coexistir com `/?cliente` sem ser tratado primeiro como um cliente chamado “login”.

Ao reorganizar a árvore, teste esse caso de colisão. A ordem de declaração de dois nós dinâmicos também pode mudar qual chave será capturada.

## Nós aninhados

Para `/clientes/financeiro`, a convenção pode chegar a `app\controllers\Clientes\Financeiro`: a pasta `Clientes` e a classe `Financeiro.php`. O Router remove o `Index` inicial ao avançar pela estrutura; não significa que todas as classes ancestrais sejam instanciadas.

A resolução produz uma classe final a ser atendida. Não há execução automática de controllers pais como uma cadeia de middlewares.

## Alterar namespace e controller

`namespace` modifica a base que o Router usa para compor o caminho da classe. `controller` substitui o nome terminal do controller selecionado.

```php
'/clientes' => [
    'namespace' => 'app\\controllers\\administracao',
    'controller' => 'Cadastro',
    '@main' => [],
],
```

Nesse exemplo, a classe pretendida é `app\controllers\administracao\Cadastro`. No literal PHP com aspas simples, `\\` representa a barra invertida usada no namespace. Confira o caminho e a declaração real da classe, especialmente em Linux.

Não use um namespace com separadores de diretório `/`. A Definition possui uma expressão para o formato de namespaces PHP.

## Aplicação em subdiretório

Se a aplicação é acessada sob `/sistema`, a primeira chave externa pode ser `'/sistema'`, e `getBaseHref()` retornará `/sistema/`. O servidor também precisa encaminhar essa localização ao front controller correto; mudar a chave não cria sozinho um Alias, um VirtualHost ou uma localização Nginx.

`removePrefixOnce()` compara o prefixo e o limite de segmento. Ele evita retirar `/sistema` de `/sistemas`, e a query string é removida antes dessa operação. Não generalize isso para uma garantia de validação de host ou de implantação: o Router continua trabalhando com o caminho recebido do servidor.

## Limites dos getters

`Router::getDynamicNode()` acessa diretamente a chave e declara retorno `string`. Uma chave inexistente não tem um fallback documentado como ocorre com `getParameter()`. Consulte os nomes capturados ou use `getDynamicNodeValues()` e `array_key_exists()` quando precisar verificar presença.

## Onde conferir no código

- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/core/config/definitions/route.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/definitions/route.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
