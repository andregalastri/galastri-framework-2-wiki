> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Laboratório File — arquivo físico e conteúdo gerado

## Dois caminhos de entrada

File pode ler um arquivo dentro de uma pasta configurada ou receber bytes já produzidos por um controller. Esses caminhos convergem na entrega, mas não recebem as mesmas verificações antes dela.

## Caminho 1: servir um arquivo físico

Na cópia de estudo, crie `var/arquivos/manual.txt` com um texto simples. A pasta fica fora de `public`. Confira em `app/config/mime-types.php` o mapeamento de `txt` e os tipos aceitos.

Adicione este nó à raiz de `routes.php`:

```php
'/arquivos' => [
    'output' => 'file',
    'fileFolder' => '/var/arquivos',
    'allowedFileExtensions' => ['txt'],
    'validateMimeType' => true,
    'isDownloadableFile' => false,
],
```

Esse caso pode funcionar sem `app\controllers\Arquivos`. Acesse `/arquivos/manual.txt`. O caminho restante da URL é usado para localizar o arquivo dentro de `fileFolder`.

O exemplo mantém a verificação de MIME. Se `txt` não estiver mapeado para o tipo detectado no seu sistema, ajuste o mapeamento local conscientemente e confira o conteúdo real; não desative a verificação apenas para ocultar o diagnóstico.

Para conferir o bloqueio de travessia, use `curl --path-as-is` com um caminho que tente subir de pasta, inclusive codificado. O resultado esperado é 404, sem retornar um arquivo externo. Não use arquivos com segredos como conteúdo de laboratório.

## Caminho 2: gerar um download no controller

Adicione um nó `/relatorio` com saída `file` e `@main`. Crie `app/controllers/Relatorio.php`:

```php
<?php
namespace app\controllers;

use galastri\modules\Controller;

final class Relatorio extends Controller
{
    protected function main(): array
    {
        $contents = "nome;quantidade\nExemplo;2\n";
        $this->setFileContents($contents, 'Relatório de exemplo.csv', 'text/csv; charset=utf-8');
        $this->setDownloadableFile(true);

        return [];
    }
}
```

`setFileContents()` recebe bytes, nome e MIME. Não recebe um caminho para que File o abra. Como o conteúdo já existe, File não passa pelo mesmo caminho de `fileFolder`, extensão e `mime_content_type()` do caso anterior.

## Conferir nome e cabeçalhos

```bash
curl -sS -D - -o /dev/null http://127.0.0.1:8080/relatorio
```

O cabeçalho `Content-Disposition` deve conter um nome ASCII de compatibilidade e `filename*` com o nome UTF-8 codificado. O navegador pode usar o nome acentuado conforme seu suporte ao parâmetro.

O download usa `application/octet-stream`. Na renderização comum, o MIME fornecido ou detectado participa do Content-Type. `Content-Length` do download é calculado em bytes, não em quantidade de caracteres visíveis.

## O que a rota não faz sozinha

Servir um arquivo não verifica o direito de uma pessoa acessar aquele documento específico. `authTag` pode exigir autenticação, e a aplicação pode precisar de uma verificação adicional de permissão e propriedade do arquivo.

O módulo carrega o arquivo inteiro em memória com `file_get_contents()`. Ele não implementa neste caminho download por blocos nem suporte completo a Range. Avalie esse contrato antes de usar arquivos muito grandes.

## Onde conferir no código

- [`galastri/core/output/File.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/File.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)
- [`app/config/mime-types.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/config/mime-types.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
