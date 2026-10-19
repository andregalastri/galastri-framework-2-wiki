> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Router — algoritmo de resolução e pontos de manutenção

## Entrada e estado produzido

`Router::run()` importa `app/config/routes.php`, resolve a raiz, separa partes da URL, percorre nós, aplica o controller terminal, seleciona o endpoint e distribui parâmetros.

As principais propriedades estáticas guardam o restante do caminho, o namespace acumulado, o método resolvido, capturas dinâmicas e a flag de validade. Elas são resultados intermediários do algoritmo; alterar uma etapa pode mudar o que as seguintes recebem.

## Métodos internos em sequência

| Método | Trabalho |
| --- | --- |
| `configureRootDir()` | Usa a primeira chave externa e verifica seu formato de caminho. |
| `setUrlParts()` | Retira a query string, remove o prefixo da raiz quando corresponde e divide segmentos. |
| `resolveNodes()` | Procura nó fixo, preserva endpoint reconhecido ou escolhe nó dinâmico. |
| `processNodeData()` | Consome um segmento, aplica configurações, compõe namespace e continua a descida. |
| `configureNodeProperties()` | Aplica a configuração de controller ao nó terminal. |
| `resolveEndpoints()` | Seleciona o endpoint, aplica suas configurações e resolve o método PHP. |
| `configureEndpointProperties()` | Aplica `method`, `httpMethod`, `parameters` e `viewFile`. |
| `resolveParameters()` | Consome valores, verifica obrigatórios e detecta excesso. |

## Nomes diferentes, responsabilidades diferentes

O texto `@meus-dados` pode virar `meusDados()` pela conversão do nome do endpoint. `method` permite escolher um nome específico, e o resultado final é mantido em `Router::$methodName`. O núcleo usa esse resultado por `getMethodName()`.

`httpMethod` é um mapa independente de verbos para métodos adicionais. Seus valores têm `@` inicial removido, mas não passam por toda a conversão de um endpoint. Use nomes válidos de métodos PHP nesse mapa.

## Configurações não são aplicadas por reflexão

`configureRouteProperties()` chama explicitamente `setConfigIfExists()` para uma lista de nomes. Criar uma nova Definition com `context => 'route'` não faz o Router passar a ler automaticamente uma chave de mesmo nome em `routes.php`.

A propriedade `context` é validada e consultável na Definition, mas não é um fiscal genérico de posição na árvore. Para documentar uma nova configuração, confira tanto o contrato quanto o ponto real de consumo.

## Parâmetros e sobras

O algoritmo divide a string `parameters` e consome valores com `array_shift()`. Um obrigatório ausente marca a rota inválida; um opcional ausente fica nulo. Ao final, qualquer segmento não consumido também invalida a rota.

Os nomes são chaves, não posições de argumentos de método. Prefira nomes textuais como `id`; a filtragem padrão com `array_filter()` também elimina uma string `'0'` da lista de nomes, embora o formato da definição aceite dígitos.

Para saída File, um restante de caminho pode representar o arquivo físico. `Galastri` só interrompe pela flag de rota inválida quando a saída exige controller. Não remova as sobras indiscriminadamente ao corrigir parâmetros: elas também alimentam a resolução do arquivo.

## Getters públicos

| Consulta | Retorno e observação |
| --- | --- |
| `getUrlParts()` | Array do caminho ainda não consumido. |
| `getControllerName()` | Nome terminal da classe. |
| `getControllerNamespace()` | Array de segmentos do namespace. |
| `getMethodName()` | Método PHP já resolvido. |
| `getDynamicNodeValues()` | Array com capturas dinâmicas. |
| `getDynamicNode($key)` | String de uma captura existente. |
| `getParameterValues()` | Array com parâmetros resolvidos. |
| `getParameter($key)` | String ou `null`. |
| `getRootDir()` | Raiz de URL configurada. |
| `getBaseHref()` | Raiz normalizada com uma barra final. |
| `isValidRoute()` | Flag de validade do percurso. |

## Casos essenciais para uma alteração

Compare raiz simples e subdiretório; URI com query string; nó fixo contra dinâmico; endpoint padrão e explícito; parâmetro ausente, opcional e excedente; e entrega File sem controller. Use um processo ou request independente para cada cenário.

## Onde conferir no código

- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
