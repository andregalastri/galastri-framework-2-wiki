> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Json — serialização e estado do streaming

## Serialização comum

`Json::run()` obtém a resposta do controller, serializa com `json_encode()` e aplica a política de cache de autenticação. Envia `application/json` quando cabeçalhos ainda podem ser definidos e imprime o resultado.

As flags são `JSON_UNESCAPED_SLASHES`, `JSON_UNESCAPED_UNICODE`, `JSON_INVALID_UTF8_SUBSTITUTE` e `JSON_THROW_ON_ERROR`. UTF-8 inválido recebe substituição; outras falhas de codificação podem lançar `JsonException`, como uma referência recursiva não codificável.

Não use `echo json_encode(...)` no controller para substituir esse caminho. Além de duplicar a serialização, isso pode resultar em dois documentos JSON no mesmo corpo.

## run depois de stream

Quando `isStreaming()` é verdadeiro, `run()` mantém `application/x-ndjson; charset=utf-8`, imprime o JSON final seguido de `\n` e chama `flush()`. A resposta final continua existindo; `ResponseStream::send()` não a substitui.

O array final vem da composição normal do controller, enquanto cada envio intermediário contém somente o payload informado ao streaming.

## A primeira chamada de stream

`stream(array $data)` serializa o payload antes de iniciar a transmissão. Na primeira chamada, aplica cache de autenticação, tenta definir Content-Type e `X-Accel-Buffering: no`, e encerra os buffers PHP removíveis usando `ob_end_flush()`.

Somente depois desse preparo a flag estática `streaming` é ativada. O método imprime a linha e chama `flush()`. Chamadas seguintes não repetem toda a inicialização.

`ob_end_flush()` descarrega o conteúdo anterior do buffer em vez de apagá-lo. Se já existia HTML ou outro texto acumulado, esses bytes também podem ir para o cliente. Portanto, um fluxo NDJSON deve começar sem saída estranha anterior.

## Limite observado no ramo de erro de buffer

Os ramos que tentam lançar uma `Exception` quando um buffer não pode ser encerrado usam um nome de classe sem importação correspondente neste arquivo. Nesse caso específico, a resolução pode procurar `galastri\core\output\Exception` e produzir erro de classe ausente, em vez da exceção pretendida.

Esta página registra o comportamento do commit; não o corrige. O caso é listado em [Limites conhecidos](/andregalastri/galastri-framework-2/wiki/Limites-conhecidos) para revisão humana.

## API pública

| Método | Efeito |
| --- | --- |
| `run(): void` | Emite a resposta final normal ou a linha final do stream. |
| `stream(array $data): void` | Inicializa o streaming quando necessário e transmite uma linha. |
| `isStreaming(): bool` | Consulta o estado estático. |
| `requiresController(): bool` | Retorna `true`. |

## O que uma mudança precisa preservar

Uma linha deve continuar sendo um JSON completo e compacto. Acentos não podem ser quebrados pelo cliente na divisão arbitrária de chunks. A transmissão final e o tratamento de erro depois do primeiro envio precisam manter o mesmo protocolo.

Leia a implementação junto de ErrorHandler, que consulta `isStreaming()` para decidir como apresentar uma falha JSON.

## Onde conferir no código

- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/modules/ResponseStream.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/ResponseStream.php)
- [`galastri/core/ErrorHandler.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
