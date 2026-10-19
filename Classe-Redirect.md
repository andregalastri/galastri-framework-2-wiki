> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Redirect — destino, raiz de URL e encerramento

## A operação principal

`Redirect::to($location, $replacements = [])` aplica placeholders, normaliza o destino relativo, envia `Location` com o status escolhido e chama `exit()`. O fluxo normal do controller e da saída não continua depois dessa chamada.

O status padrão é 302. `statusCode($code)` aceita a lista implementada: 300, 301, 302, 303, 307 e 308. O método retorna o nome da classe para permitir o encadeamento estático adotado pelo módulo.

## Raiz da aplicação

Para um destino sem um dos protocolos reconhecidos, o módulo acrescenta uma barra inicial e, por padrão, a raiz obtida pelo Router. Se a aplicação estiver sob `/sistema`, `to('/clientes')` produz um destino sob `/sistema/clientes`.

`ignoreRootDir()` ativa um estado estático que evita esse prefixo. Um destino vazio é tratado como `/`. A lista de protocolos reconhecidos inclui mais que HTTP e HTTPS; reconhecer um esquema não prova que o navegador aceitará usá-lo num cabeçalho Location.

## Placeholders sem conflito com percentuais

```php
Redirect::statusCode(303)::to(
    '/clientes/detalhes/{{id}}?origem=cadastro',
    ['id' => rawurlencode((string) $id)]
);
```

O trecho assume um controller já em execução e um identificador obtido de fonte apropriada. O 303 é uma escolha comum quando a aplicação quer que o cliente consulte outro recurso depois de uma operação; a semântica precisa ser coerente com seu fluxo HTTP.

URLs com `%20` não são templates `printf`. A substituição é feita pelos marcadores `{{...}}`. O módulo não faz por si `rawurlencode()` de cada valor substituído; a aplicação escolhe a codificação correta para segmento ou query string.

## Destino controlado

O método não implementa uma lista de destinos externos permitidos. Passar uma URL livre recebida do usuário pode permitir redirecionamento para outro site. Para fluxos de retorno, selecione destinos locais previstos ou aplique uma política explícita de validação.

## Estado e testes

As opções de status e raiz ficam em propriedades estáticas até o fim da requisição; não são objetos novos criados por cada chamada fluente. Como `to()` encerra o processo, teste destinos por requisições ou processos separados.

No curl, confira o cabeçalho sem seguir o redirecionamento automaticamente. Casos úteis incluem raiz `/`, subdiretório, destino externo, valor com `%`, marcador escapado e status fora da lista.

## Onde conferir no código

- [`galastri/modules/Redirect.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Redirect.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
