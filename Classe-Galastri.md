> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Galastri — orquestração da requisição

## Responsabilidade

`galastri\core\Galastri` é uma classe final e estática. Ela coordena o atendimento e mantém os nomes de controller e métodos que serão usados nas próximas etapas.

## Ordem exata de run

1. `Config::run()` carrega as definições.
2. `Router::run()` interpreta a requisição e aplica configurações.
3. Define timezone e classe de saída.
4. Verifica `forceRedirectTo` e modo offline.
5. Verifica a validade da rota, quando a saída exige controller.
6. Verifica autenticação, se `authTag` não estiver vazia.
7. Resolve e verifica a classe do controller.
8. Resolve e verifica o método do endpoint.
9. Resolve e verifica o método adicional do verbo HTTP.
10. Instancia o controller, se ele existir.
11. Executa `run()` da classe de saída.

A posição das verificações é parte do comportamento. Um redirecionamento forçado encerra o fluxo antes de instanciar controller; um modo offline pode interromper a execução antes da autenticação.

## Escolha da saída

`setOutputClass()` converte o valor configurado para PascalCase e monta `galastri\core\output\View`, `Json` ou `File`. A Definition restringe os valores a `view`, `json` e `file`.

A classe escolhida fica armazenada em `Galastri::$output`. Alterar `Config::set('output', ...)` depois dessa seleção não substitui essa referência. O controller não oferece setter de formato; definir a saída na rota mantém o atendimento consistente.

## Controller opcional na saída File

`View::requiresController()` e `Json::requiresController()` retornam `true`. `File::requiresController()` retorna `false` porque pode servir um arquivo físico sem classe da aplicação.

Se existir uma classe no caminho resolvido para uma rota File, ela ainda passa pela verificação de herança e métodos. A saída opcional não significa que uma classe incorreta seja silenciosamente ignorada.

## Método de endpoint e método HTTP

O método do endpoint vem de `Router::getMethodName()`. O método adicional vem da configuração `httpMethod`, usando o verbo de `$_SERVER['REQUEST_METHOD']` em minúsculas.

Quando há correspondência, o construtor do controller executa **primeiro o endpoint, depois o método HTTP**. Não se trata de escolher um ou outro. Se não houver uma entrada para o verbo recebido, o método adicional não é executado; o núcleo não produz automaticamente um `405 Method Not Allowed`.

## Falhas e redirecionamentos

`notFound()` define HTTP 404 e lança a exceção configurada, salvo quando há redirecionamento aplicável. Para saída JSON, os redirecionamentos de “não encontrado” e falha de autenticação são evitados pelo fluxo específico; uma falha gera a apresentação de erro.

Uma falha de autenticação lança exceção, mas não define por si um 401 ou 403 nesta rotina. O ErrorHandler normalmente usará 500 se nenhum status de erro tiver sido definido. Essa é uma característica observada do código, não uma recomendação de semântica HTTP.

## Métodos públicos de consulta

| Grupo | Métodos | Contrato |
| --- | --- | --- |
| Existência | `controllerExists()`, `methodExists()`, `httpMethodExists()` | Informam flags preparadas durante a resolução. |
| Identidade | `getControllerNamespace()`, `getControllerMethodName()`, `getControllerHttpMethodName()` | Strings resolvidas para a execução. |
| Conteúdo | `getControllerResponse()`, `getControllerFileContents()` | Delegam ao objeto de controller; exigem que ele exista. |
| Execução | `run()`, `notFound()` | Coordenam ou interrompem o atendimento. |

## Ao modificar esta classe

Teste ao menos uma saída que exige controller, uma rota File sem controller, um endpoint com mapeamento HTTP e uma falha de rota. Mudar a ordem dessas etapas pode alterar o que aparece no erro, se uma sessão é iniciada e se bytes já foram enviados.

## Onde conferir no código

- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
