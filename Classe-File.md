> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# File — resolução segura de caminho e entrega

## Ordem de execução

`run()` procura primeiro conteúdo fornecido pelo controller. Se o array estiver vazio, resolve pasta, arquivo, extensão, MIME e conteúdo do disco. Por fim, escolhe download ou renderização de acordo com `isDownloadableFile`.

File não exige controller, mas pode utilizá-lo quando existir. O `requiresController(): bool` retorna `false`.

## Como o caminho físico é delimitado

1. `fileFolder` precisa estar configurado.
2. Os segmentos restantes da URL são decodificados com `rawurldecode()` e reunidos.
3. Uma string vazia, um byte nulo ou uma pasta-base inexistente resultam em “não encontrado”.
4. `realpath()` resolve a pasta e o candidato, incluindo `..` e links simbólicos.
5. O candidato precisa ser um arquivo e começar pelo prefixo da pasta-base com separador de diretório.

O separador no prefixo importa: uma pasta `/var/documentos-extra` não deve passar por começar textualmente com `/var/documentos`. Um link simbólico que resolve fora da base também deve ser recusado.

Essa validação pertence ao caminho de arquivo físico. Não valide uma URL externa com esse método como se ele fosse uma política genérica de armazenamento.

## Extensão e MIME

A extensão vem de `pathinfo()`. Uma lista `allowedFileExtensions` não vazia restringe os valores aceitos. A comparação não normaliza automaticamente a caixa: uma extensão maiúscula pode diferir da minúscula configurada.

O MIME é obtido do mapa `forceMimeType` para a extensão, quando presente; caso contrário, de `mime_content_type()`. Quando `validateMimeType` está ativo, a extensão precisa existir em `MIME_TYPE_LIST` e o MIME selecionado precisa constar de sua lista.

Como `forceMimeType` atua antes da validação, ele muda também o valor comparado à lista. Não o apresente como uma segunda inspeção independente do conteúdo real.

## Conteúdo fornecido pelo controller

`getControllerFileContents()` recebe o array montado por `setFileContents()`. Quando há conteúdo nesse array, o caminho de resolução física é pulado. A aplicação passa a responder pela origem dos bytes, pelo MIME informado e pelas verificações de negócio correspondentes.

O nome de download passa por higienização mesmo nesse caminho: separadores e caracteres de controle são substituídos; nomes vazios, `.` e `..` recebem fallback. Um segundo nome ASCII é preparado para compatibilidade e o nome UTF-8 é enviado por `filename*`.

## Renderização, download e memória

A renderização consulta BrowserCache usando conteúdo e nome antes de imprimir. O download aplica a política de autenticação, envia descrição, disposition, expiração e tamanho, e imprime os bytes.

O tamanho usa `mb_strlen(..., '8bit')`, portanto conta bytes. O conteúdo permanece integralmente em memória. A existência de chamadas a `flush()` não transforma o carregamento do arquivo em streaming do disco.

## Casos de regressão

Inclua arquivo válido, inexistente, extensão proibida, MIME não configurado, nome com acento, nome com caracteres de controle, caminho codificado, byte nulo e link simbólico externo. Use conteúdos inofensivos e confira tanto status quanto corpo.

Ao mudar a decodificação da URL, teste também `%2F`, `%2E%2E` e nomes reais com espaços. Nem todo teste deve terminar em acesso permitido: o resultado depende do caminho final resolvido dentro da base.

## Onde conferir no código

- [`galastri/core/output/File.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/File.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
