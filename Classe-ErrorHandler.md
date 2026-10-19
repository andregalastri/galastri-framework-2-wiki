> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# ErrorHandler — erro PHP, exceção, resposta e log

## Três entradas para um mesmo fluxo

`ErrorHandler::run()` registra um handler de erros PHP, um handler de exceções não capturadas e uma função de shutdown. O primeiro respeita a máscara de `error_reporting()`; o último procura os tipos de falha fatal listados na implementação.

A instalação acontece no bootstrap, depois de constantes, autoload e alias de VarDump. Um problema ocorrido antes dessa etapa pode não receber o tratamento do framework.

## A estrutura da apresentação

O array de erro contém `date`, `time`, `code`, `origin`, `line`, `message`, `trace`, `warning` e `error`. Nesta implementação, os dois últimos campos são definidos como `true` na montagem, inclusive para falhas que não sejam um simples aviso de negócio.

Isso não é o mesmo array obrigatório de todos os sucessos. Também não inclui automaticamente os dados adicionais guardados pela exceção personalizada.

## Privacidade de detalhes e mensagens de validação

Com `displayErrors` ativo, a mensagem técnica e a origem podem aparecer. Trace ainda exige `showTrace`. Sem essa liberação, a origem fica nula, a linha zero e o trace vazio; a mensagem normalmente é a mensagem genérica do catálogo.

A exceção personalizada pode carregar `displayError` para liberar **sua mensagem**. O mecanismo de validação utiliza isso para mostrar uma falha compreensível ao usuário. Esse sinal não libera sozinho arquivo, linha ou trace.

## Escolha do status HTTP

| Situação | Decisão |
| --- | --- |
| Um status entre 400 e 599 já foi definido | Preserva o status existente. |
| Aviso ou notice reconhecido pelo mapa de status | Usa 400 quando não há erro já definido. |
| Demais falhas | Usa 500 quando não há erro já definido. |
| Cabeçalhos já enviados | Não consegue substituir o status pelo mesmo caminho. |

Um código de exceção como `PERMISSION_DENIED` não é convertido automaticamente para 403. A regra de status é separada do identificador de erro.

## Apresentação conforme a saída

| Saída configurada | Forma |
| --- | --- |
| `json` antes do streaming | Objeto JSON com formatação legível. |
| `json` depois do streaming | Uma linha JSON por `Json::stream()`. |
| `file` | Imagem PNG montada com GD, fonte e logo internos. |
| `view` ou fallback | Bloco HTML `<pre>` com os dados escapados. |

Por isso GD faz parte dos requisitos do projeto: não é usado apenas por uma eventual aplicação de imagens. A apresentação File depende também do suporte à fonte e dos recursos em `galastri/misc/`.

No PHP 8.5, `imagedestroy()` é depreciado. A implementação atual ainda o chama durante a apresentação File. Um aviso emitido antes de enviar o cabeçalho PNG pode contaminar o corpo e impedir a resposta gráfica correta; esse ramo exige revisão específica no ambiente nativo. [Referência oficial do PHP](https://www.php.net/manual/en/function.imagedestroy.php).

O runtime isolado usado nesta edição possui GD, mas não suporte FreeType. Por isso ele confirmou a dependência e os avisos, sem validar a apresentação PNG final. Os testes de contenção de arquivos verificaram status e ausência do conteúdo externo, não a aparência gráfica do erro. [Como consultar os recursos de GD](https://www.php.net/manual/en/function.gd-info.php).


## Logs

Se `createLogsOnError` estiver ativo, o framework escreve `logs/ErrorHandler-AAAA-MM-DD.log`, com a informação técnica original. A implementação insere a ocorrência no início e solicita permissões restritas para novos arquivos.

A apresentação ocorrerá antes da tentativa de escrever o log. Um erro de permissão no destino pode, portanto, causar outra falha durante o tratamento. Confira o usuário PHP-FPM e a gravabilidade de `logs/` ao preparar o ambiente.

## Avisos e saída já produzida

A implementação não limpa universalmente toda saída anterior a cada falha. Se um controller já imprimiu HTML, esse trecho pode continuar presente antes do JSON de erro. Com `stopOnWarnings => false`, certos avisos também podem ser apresentados e a execução prosseguir, resultando em conteúdo misturado.

O handler é uma estratégia de diagnóstico e apresentação; não uma garantia de desfazer bytes já enviados, transações, gravações em arquivo ou qualquer efeito colateral anterior.

## Experiência de manutenção

Numa rota JSON local, defina HTTP 422 e lance uma exceção. Confira a preservação do status. Repita sem definir status e compare. Depois faça uma falha posterior ao primeiro `ResponseStream::send()` e veja a linha de erro no mesmo protocolo NDJSON.

Use mensagens fictícias e não publique traces de sistemas reais na Wiki.

## Onde conferir no código

- [`galastri/core/ErrorHandler.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
