> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Bootstrap, constantes e autoload

## Ordem de execução

| Ordem | Operação | Consequência |
| --- | --- | --- |
| 1 | `ob_start()` | Inicia um buffer PHP para a saída posterior. |
| 2 | Inclui `constants.php` e `functions.php` | Disponibiliza caminhos e auxiliares básicos. |
| 3 | Inclui `vendor/autoload.php` | Permite localizar classes pelos namespaces. |
| 4 | Cria o alias `\VarDump` | Disponibiliza o atalho de depuração. |
| 5 | `ErrorHandler::run()` | Registra handlers de erro, exceção e shutdown. |
| 6 | Inclui `app/bootstrap.php`, se existir | Executa preparação própria da aplicação. |
| 7 | `Galastri::run()` | Inicia configurações, rotas, controller e resposta. |

`app/bootstrap.php` executa **antes** de `Config::run()` e `Router::run()`. Portanto, não use nele um `Config::get()` sem considerar que as definições ainda não foram registradas. Também não use `Router::getRootDir()` como se a rota já estivesse resolvida.

## Constantes

| Nome | Valor ou origem | Uso |
| --- | --- | --- |
| `PROJECT_DIR` | Caminho real da raiz do projeto | Base para localizar configuração, autoload, views e logs. |
| `PERFORMANCE_ANALYSIS_TAG` | 10 bytes aleatórios em hexadecimal | Identificador disponível; não comprova um analisador de performance ativo. |
| `GALASTRI_VERSION` | Conteúdo de `galastri/VERSION`, quando existe | Metadado da resposta. |
| `APP_VERSION` | Conteúdo de `app/VERSION`, quando existe | Metadado da aplicação. |
| `MIME_TYPE_LIST` | Retorno de `app/config/mime-types.php`, ou `[]` | Validação de MIME na saída File. |
| `RESET_BACKTRACE_LEVEL` / `KEEP_BACKTRACE_LEVEL` | `true` / `false` | Opções legíveis para o VarDump. |
| `INSERT_CONTENT_AT_START` | `1` | Inserção no começo em `Tools::writeFile()`. |
| `INSERT_CONTENT_AT_END` | `2` | Inserção no final. |
| `OVERWRITE_CONTENT` | `3` | Substituição do conteúdo. |

Essas constantes são definidas ao incluir o arquivo. Incluí-lo repetidamente no mesmo processo pode produzir avisos de redefinição; em testes, prepare o ambiente uma vez ou use processos separados.

## Autoload não é bootstrap

Para testar `Tools` ou um validador isolado, normalmente basta definir as constantes exigidas e carregar o autoload. Incluir o bootstrap completo também registra handlers e atende uma requisição; isso pode terminar o processo, enviar cabeçalhos e depender de variáveis `$_SERVER`.

Um ponto de partida para **`teste.php` na raiz local** é:

```php
<?php
require __DIR__.'/galastri/bootstrap/constants.php';
require __DIR__.'/vendor/autoload.php';

use galastri\modules\Tools;

echo Tools::replacePlaceholders('Olá, {{nome}}!', ['nome' => 'Colaborador']);
```

Resultado esperado: `Olá, Colaborador!`. Esse teste não afirma que o roteamento ou o servidor estejam configurados; ele verifica somente um componente carregado pelo autoload.

## O papel do buffer

`ob_start()` armazena saída em memória no PHP. O código atual de `ErrorHandler` imprime a apresentação do erro, mas não possui uma limpeza geral de todos os buffers antes de cada resposta. Portanto, não prometa que um `echo` feito anteriormente será apagado automaticamente quando uma exceção ocorrer.

O streaming encerra buffers removíveis ao começar. Depois de enviar bytes, o framework não consegue “voltar no tempo” para trocar o status HTTP já enviado ou recolher uma linha de progresso. Estude esse limite no capítulo de streaming.

## Função vardump

`functions.php` declara `vardump()`, um wrapper que ajusta a referência de chamada e aciona o VarDump do framework. Ele produz apresentação HTML; não deve ser usado dentro de uma resposta JSON ou NDJSON que precise continuar válida.

## Onde conferir no código

- [`galastri/bootstrap/bootstrap.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/bootstrap/bootstrap.php)
- [`galastri/bootstrap/constants.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/bootstrap/constants.php)
- [`galastri/bootstrap/functions.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/bootstrap/functions.php)
- [`galastri/core/ErrorHandler.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
