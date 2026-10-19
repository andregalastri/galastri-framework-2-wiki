> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# ResponseStream — progresso em NDJSON do PHP ao navegador

## O que este módulo oferece

`galastri\modules\ResponseStream` é uma fachada estática com `send(array $data): void`. Ela chama `Json::stream($data)`. Não mantém uma cópia do retorno do controller e não acrescenta campos de negócio ao payload.

O transporte adotado é **NDJSON**: um documento JSON completo por linha. Continua sendo a saída `json` da rota; não há um quarto valor de output chamado `stream`.

## Laboratório PHP

Configure uma rota `/processar` com `output => 'json'`, `includeProjectMetadata => false` e `@main`. Crie `app/controllers/Processar.php`:

```php
<?php
namespace app\controllers;

use galastri\modules\Controller;
use galastri\modules\ResponseStream;

final class Processar extends Controller
{
    protected function main(): array
    {
        ResponseStream::send([
            'type' => 'progress',
            'message' => 'Validando os dados...',
            'progress' => 10,
        ]);

        sleep(1);

        ResponseStream::send([
            'type' => 'progress',
            'message' => 'Preparando o resultado...',
            'progress' => 70,
        ]);

        sleep(1);

        return [
            'type' => 'result',
            'error' => false,
            'warning' => false,
            'code' => 'PROCESS_FINISHED',
            'message' => 'Processamento concluído.',
            'data' => ['progress' => 100],
        ];
    }
}
```

As pausas são artificiais para observar a transmissão. Elas não fazem parte da técnica de processamento real.

## Observar antes de criar a interface

```bash
curl -N -i http://127.0.0.1:8080/processar
```

`-N` desativa o buffer de saída do curl. Espere `application/x-ndjson; charset=utf-8` e três linhas JSON: duas intermediárias e uma final. O tipo `progress` e o percentual são um contrato desse laboratório, não uma estrutura imposta pelo módulo.

Etapas muito rápidas podem chegar juntas. Uma chamada a `send()` é uma unidade lógica de mensagem, não uma garantia de pacote TCP separado nem de atualização visual imediata.

## Cliente JavaScript que respeita os limites de linha

```javascript
function tratarMensagem(payload) {
    if (payload.error) {
        console.error(payload.message);
        return;
    }

    console.log(payload);
}

function consumirLinhas(state, text) {
    state.pending += text;
    let newline = state.pending.indexOf('\n');

    while (newline !== -1) {
        const line = state.pending.slice(0, newline).trim();
        state.pending = state.pending.slice(newline + 1);

        if (line !== '') {
            tratarMensagem(JSON.parse(line));
        }

        newline = state.pending.indexOf('\n');
    }
}

async function acompanharProcessamento() {
    const response = await fetch('/processar');
    const contentType = (response.headers.get('Content-Type') || '').split(';')[0].trim();

    if (contentType === 'application/json') {
        const payload = await response.json();
        tratarMensagem(payload);
        return;
    }

    if (contentType !== 'application/x-ndjson') {
        throw new Error('A resposta não usa o formato esperado.');
    }

    if (!response.body) {
        throw new Error('O navegador não disponibilizou o corpo em streaming.');
    }

    const reader = response.body.getReader();
    const decoder = new TextDecoder('utf-8');
    const state = {pending: ''};

    try {
        while (true) {
            const chunk = await reader.read();

            if (chunk.done) {
                break;
            }

            consumirLinhas(state, decoder.decode(chunk.value, {stream: true}));
        }

        consumirLinhas(state, decoder.decode());

        if (state.pending.trim() !== '') {
            tratarMensagem(JSON.parse(state.pending));
        }
    } finally {
        reader.releaseLock();
    }
}
```

O decoder preserva caracteres UTF-8 divididos entre chunks. O texto pendente preserva uma linha JSON incompleta. O laço também aceita várias linhas no mesmo chunk.

O caminho `application/json` é importante para um erro antes do primeiro envio. O exemplo mostra o parsing; uma interface real também deve decidir o que fazer com `response.status`, cancelamento e exceções do leitor. O endpoint relativo deve ser ajustado se a aplicação estiver montada sob um subdiretório.

## Erros e confirmação final

Antes do primeiro envio, ainda é possível definir um status de erro e responder JSON comum. Depois que cabeçalhos e bytes já foram transmitidos, o ErrorHandler pode enviar uma linha JSON de erro, mas não trocar com confiança o status HTTP já recebido.

O cliente não deve concluir “operação bem-sucedida” apenas porque recebeu progresso ou HTTP 200. Aguarde um resultado final definido pelo seu contrato; uma conexão encerrada antes dele pode representar interrupção.

## Buffers no caminho

| Camada | O que conferir |
| --- | --- |
| PHP | Buffers abertos, `output_buffering`, saída anterior ao primeiro envio. |
| PHP-FPM e Apache | Socket correto e política de descarga FastCGI. |
| Nginx | `fastcgi_buffering`, compressão e cabeçalho `X-Accel-Buffering`. |
| Proxy adicional | Se acumula ou comprime a resposta. |
| Cliente | Buffer do curl e parser por linha do navegador. |

Comece com um caso pequeno e pausas visíveis. Não acrescente caracteres de preenchimento ao protocolo como primeira tentativa de corrigir uma camada de servidor.

## Onde conferir no código

- [`galastri/modules/ResponseStream.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/ResponseStream.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/core/ErrorHandler.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
