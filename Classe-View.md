> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# View — resolução e renderização

## Ordem de run

`View::run()` resolve o arquivo de view, resolve o template e chama `render()`. A view é exigida mesmo quando o template selecionado não a inclui explicitamente.

Quando `viewFile` está vazio, o nome é composto com a parte do namespace posterior a `app\controllers` e com `Galastri::getControllerMethodName()`. A pasta-base vem de `viewFolder`.

O template é obtido de `templateFile` a partir de `PROJECT_DIR`. Se a configuração estiver vazia, `render()` inclui diretamente a view. Um caminho configurado, mas inexistente, produz exceção com mensagem própria.

## O buffer da renderização

`render()` registra o nível de buffer existente e inicia um novo buffer. Cria `$g`, inclui o arquivo selecionado e captura o conteúdo renderizado. Em caso de `Throwable`, tenta encerrar os buffers removíveis que foram abertos acima do nível inicial e relança a falha.

Isso evita que a própria etapa de renderização deixe indiscriminadamente buffers adicionais após uma falha. Não é uma transação que desfaz todo efeito do PHP: uma escrita em arquivo ou uma query realizada pelo template não é revertida pelo encerramento do buffer.

## Cache depois da geração

O conteúdo já renderizado é passado a `checkBrowserCache()`. Se a trait reconhecer o ETag e produzir 304, View retorna sem imprimir o corpo. O controller e o template já foram executados nesse caminho.

Esse é um cache HTTP para o cliente, não um cache de resultado que elimina a computação da aplicação. Ao avaliar performance, não atribua a ele uma economia de consultas que o código não implementa.

## Dependências e API

| Elemento | Papel |
| --- | --- |
| `View::$viewPath` | Caminho público estático consultado por `PhpEngine::include('view')`. |
| `run(): void` | Resolução e apresentação. |
| `requiresController(): bool` | Retorna `true`. |
| `PhpEngine` | Recebe dados por Galastri e oferece get, print e include. |
| `BrowserCache` | Política de autenticação e validação de ETag na etapa final. |

A engine é escolhida por uma string interna fixa para PhpEngine. O comentário sobre engines alternativas não constitui um registro público de plugins de template pronto nesta edição.

## Ao modificar

Teste view convencional e explícita, template presente e vazio, exceção durante o include e resposta 304. Uma alteração no nome do método ou namespace pode quebrar a convenção de arquivos sem alterar a classe View diretamente.

## Onde conferir no código

- [`galastri/core/output/View.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/View.php)
- [`galastri/core/output/engines/PhpEngine.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/engines/PhpEngine.php)
- [`galastri/core/output/traits/BrowserCache.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/traits/BrowserCache.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
