> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Fluxo de dados — do retorno à resposta HTTP

## Dados não são ainda uma resposta pronta

Os métodos do controller retornam arrays. A base Controller reúne esses arrays e, opcionalmente, acrescenta metadados. Só depois a classe de saída selecionada transforma o resultado em bytes enviados ao cliente.

| Caminho | O que a saída consome | Como produz o corpo |
| --- | --- | --- |
| View | Array combinado do controller | Executa template/view PHP com uma instância de PhpEngine. |
| Json | Array combinado do controller | Usa `json_encode()` e imprime JSON. |
| File com conteúdo do controller | Bytes, nome e MIME fornecidos por `setFileContents()` | Renderiza ou prepara download. |
| File com arquivo físico | Pasta configurada e segmentos restantes da URL | Resolve e valida o arquivo, carrega seus bytes e entrega. |
| Streaming JSON | Arrays enviados a `ResponseStream::send()` e depois retorno final | Emite um JSON compacto por linha. |

## Não há um envelope universal de sucesso

A aplicação pode retornar `error`, `warning`, `code`, `message` e `data`, mas o núcleo não cria todos esses campos em cada retorno bem-sucedido. O laboratório JSON mostra como estabelecer esse contrato deliberadamente.

`includeProjectMetadata` acrescenta `meta` ao resultado do controller. O payload de `ResponseStream::send()` não passa automaticamente por `Controller::mergeReturns()`; ele contém apenas o array enviado.

## Cabeçalhos, status e corpo

O status HTTP é separado de um campo chamado `code` dentro do JSON. `code => 'CLIENTE_CRIADO'` não define HTTP 201; é necessário `http_response_code(201)` se esse for o contrato pretendido.

Também é possível produzir um corpo de erro com HTTP 200 se a aplicação somente retornar `error => true`. O framework não converte automaticamente toda chave de negócio em um status de transporte.

Um redirecionamento é outro caminho: `Redirect::to()` envia `Location` e encerra o processo, sem aguardar o retorno normal e a classe de saída.

## Não misturar canais por acidente

`echo`, `var_dump()` e o VarDump do framework produzem bytes imediatamente ou em buffer. Esses bytes podem ser úteis num laboratório HTML, mas deixam de ser dados do array de resposta. Em JSON e NDJSON, um trecho de HTML de diagnóstico torna o protocolo inválido.

Para observar dados sem contaminar a resposta, prefira um teste separado em `teste.php` ou um registro local apropriado. Ao investigar uma falha, sempre leia o corpo completo recebido, inclusive o que veio antes do JSON.

## Exercício

Produza o mesmo array em duas rotas, uma View e outra Json. Observe que a saída View exige os arquivos de apresentação, enquanto Json precisa apenas do array. Depois, acrescente `meta` manualmente e ative `includeProjectMetadata`: confira qual versão da chave permanece no resultado final.

## Onde conferir no código

- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)
- [`galastri/core/output/View.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/View.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/core/output/File.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/File.php)
- [`galastri/modules/ResponseStream.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/ResponseStream.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
