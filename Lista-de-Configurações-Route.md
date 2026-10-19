> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Configurações do contexto route

Os valores abaixo vêm das definições desta edição. O arquivo de rotas da aplicação pode substituir parte deles durante o percurso.

| Nome | Tipo admitido | Valor padrão | Efeito ou limite |
| --- | --- | --- | --- |
| `projectName` | `string` | `""` | Nome do projeto incluído em `meta` quando habilitado. |
| `title` | `string` | `""` | Título do atendimento; disponibilizado em `meta.title`. |
| `output` | `string` | `"view"` | Seleciona `view`, `json` ou `file` antes da execução do controller. |
| `timezone` | `string` | `date_default_timezone_get()` | Aplicado por Galastri com `date_default_timezone_set()`. |
| `offline` | `bool` | `false` | Interrompe o atendimento normal quando verdadeiro. |
| `offlineRedirectTo` | `string` | `""` | Destino usado no modo offline quando não vazio. |
| `offlineMessage` | `string` | `"System is temporarily offline."` | Mensagem da exceção quando offline sem redirecionamento. |
| `authTag` | `string` | `""` | Tag de sessão/cookie exigida pela rota; vazia não exige autenticação pelo núcleo. |
| `authFailRedirectTo` | `string` | `""` | Destino na falha de autenticação, exceto para JSON. |
| `authFailMessage` | `string` | `"Forbidden."` | Mensagem de falha de autenticação. |
| `notFoundRedirectTo` | `string` | `""` | Destino para rota não encontrada, exceto para JSON. |
| `notFoundMessage` | `string` | `"404 - Route not found"` | Mensagem da falha 404. |
| `forceRedirectTo` | `bool ou string` | `false` | Destino de redirecionamento incondicional; prefira `false` ou uma string. |
| `namespace` | `string` | `""` | Base usada na composição do namespace do controller. |
| `browserCache` | `array` | `[]` | Array com duração inteira não negativa e política de cache. |
| `templateFile` | `string` | `"/app/templates/template.php"` | Arquivo de template; vazio utiliza diretamente a view. |
| `viewFolder` | `string` | `"/app/views"` | Pasta-base das views, relativa à raiz física do projeto. |
| `fileFolder` | `string` | `""` | Pasta-base de arquivos servidos pelo output File. |
| `isDownloadableFile` | `bool` | `false` | Escolhe download com Content-Disposition em vez de renderização. |
| `allowedFileExtensions` | `array` | `[]` | Lista de extensões; vazia não restringe por extensão. |
| `permissionFailMessage` | `string` | `"Forbidden."` | Texto padrão usado por uma instância de Permission. |
| `validateMimeType` | `bool` | `true` | Valida o MIME contra MIME_TYPE_LIST para arquivos físicos. |
| `forceMimeType` | `array` | `[]` | Mapa de extensão para MIME utilizado na resolução do arquivo físico. |
| `includeProjectMetadata` | `bool` | `true` | Acrescenta `meta` ao retorno combinado do controller. |

## Aplicação e herança

Essas configurações são lidas explicitamente pelo Router durante a descida dos nós e ao entrar no endpoint selecionado. Um valor em um nível interno pode substituir o que veio da raiz. O `context` da Definition descreve sua categoria; não substitui essa lógica de leitura.

`browserCache` usa a forma `[segundos, politica]`, por exemplo `[60, 'public']`. A Definition só exige array; o consumidor verifica os componentes. Uma rota com `authTag` não vazia recebe a política `private, no-store`, mesmo que um cache público tenha sido configurado.

`forceRedirectTo` aceita `bool` no contrato, mas o uso significativo para desligar é `false`; para redirecionar, informe uma string. `true` não descreve um destino útil.

## Exemplo de aplicação

```php
<?php
return [
    '/' => [
        'projectName' => 'Laboratório',
        'timezone' => 'America/Sao_Paulo',
        '@main' => [],
        '/api' => [
            'output' => 'json',
            'includeProjectMetadata' => false,
            '@main' => [],
        ],
    ],
];
```

O exemplo ainda exige controllers compatíveis para as rotas atendidas. Configurar uma árvore não gera classes automaticamente.

## Caminhos e valores vazios

`templateFile` é um caminho a partir de `PROJECT_DIR`; o padrão aponta para um arquivo dentro da aplicação. `viewFolder` e `fileFolder` são bases físicas, não URLs. `namespace` usa barras invertidas PHP. `viewFile` pertence ao contexto endpoint.

As regex exatas estão nas definições do commit vinculado. A string vazia é tratada especialmente por Definition; para os demais valores, não substitua o contrato de caminho por uma suposição genérica.

Um arquivo fornecido diretamente com `setFileContents()` percorre outro caminho de File e não recebe todas as verificações aplicadas à leitura do disco. Veja a referência da saída antes de confiar numa configuração como proteção universal.

## Onde conferir no código

- [`galastri/core/config/definitions/route.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/definitions/route.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
