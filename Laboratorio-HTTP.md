> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Laboratório HTTP integrado — ler, validar e responder

## Objetivo

Construir um caso pequeno em que cada parte do fluxo possa ser observada: parâmetros de caminho, JSON de entrada, mapeamento HTTP, validação e composição do retorno. Use uma cópia de estudo ou preserve os arquivos dos laboratórios anteriores antes de substituí-los.

## 1. Configuração de rotas

Arquivo `app/config/routes.php`:

```php
<?php
return [
    '/' => [
        'displayErrors' => true,
        'output' => 'json',
        'includeProjectMetadata' => false,
        '@main' => [],
        '/lab' => [
            '@main' => [],
            '@soma' => ['parameters' => '/a/b'],
            '@entrada' => [
                'method' => 'prepararEntrada',
                'httpMethod' => ['POST' => '@receberEntrada'],
            ],
        ],
    ],
];
```

A aplicação de exemplo já possui `Index::main()` para a raiz. O laboratório atende os caminhos sob `/lab`.

## 2. Controller completo

Arquivo `app/controllers/Lab.php`:

```php
<?php
namespace app\controllers;

use galastri\extensions\Exception;
use galastri\modules\Controller;
use galastri\modules\HttpData;
use galastri\modules\Validation;

final class Lab extends Controller
{
    protected function main(): array
    {
        return ['code' => 'LAB_READY', 'data' => ['output' => $this->getOutput()]];
    }

    protected function soma(): array
    {
        $a = $this->getParameter('a');
        $b = $this->getParameter('b');
        $validator = Validation::number()->valueRange(-1000000, 1000000);

        try {
            $validator->validate($a);
            $validator->validate($b);
        } catch (Exception $e) {
            http_response_code(422);
            throw $e;
        }

        return ['code' => 'SUM_OK', 'data' => ['sum' => (float) $a + (float) $b]];
    }

    protected function prepararEntrada(): array
    {
        if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
            header('Allow: POST');
            http_response_code(405);
            throw new Exception('Use POST neste endpoint.');
        }

        return ['message' => 'Preparação do endpoint.', 'data' => ['origem' => 'endpoint']];
    }

    protected function receberEntrada(): array
    {
        $data = HttpData::getAll();
        $nome = $data['nome'] ?? null;

        if (!is_string($nome)) {
            http_response_code(422);
            throw new Exception('O nome deve ser uma string.');
        }

        try {
            Validation::string()
                ->lengthRange(3, 50)
                ->onError('Use um nome entre 3 e 50 caracteres.')
                ->validate($nome);
        } catch (Exception $e) {
            http_response_code(422);
            throw $e;
        }

        http_response_code(200);
        return ['code' => 'INPUT_ACCEPTED', 'message' => 'Entrada validada.', 'data' => ['nome' => $nome]];
    }
}
```

O exemplo valida a entrada e retorna HTTP 200. Um fluxo que realmente cria um recurso pode adotar HTTP 201 conforme o contrato de sua API; os campos `code` e `message` do corpo continuam separados dessa decisão.

## 3. Requisições de observação

Com o servidor local em execução:

```bash
curl -i http://127.0.0.1:8080/lab
curl -i http://127.0.0.1:8080/lab/soma/2/3
curl -i http://127.0.0.1:8080/lab/soma/2
curl -i http://127.0.0.1:8080/lab/soma/2/3/extra
curl -i http://127.0.0.1:8080/lab/entrada
curl -i -X POST -H 'Content-Type: application/json' -d '{"nome":"Ana"}' http://127.0.0.1:8080/lab/entrada
```

| Caso | Resultado a conferir |
| --- | --- |
| `/lab` | HTTP 200, código `LAB_READY` e output `json`. |
| Soma 2 e 3 | HTTP 200 e soma numérica 5. |
| Um parâmetro ausente | HTTP 404 antes do método de soma. |
| Um segmento excedente | HTTP 404. |
| GET em entrada | HTTP 405 e `Allow: POST`, definidos pelo preparador. |
| POST com Ana | HTTP 200 e o retorno do método HTTP combinado. |

Teste ainda JSON malformado, nome como array e string curta. O primeiro deve seguir o 400 de HttpData; os demais seguem os 422 que o laboratório definiu explicitamente.

## 4. Observar array_merge

No POST válido, a chave `data` do método HTTP substitui a chave `data` do preparador. O resultado final contém `nome`, sem preservar automaticamente `origem`. Isso demonstra a mesclagem superficial de Controller.

A proteção de verbo também é explícita. Se você remover a verificação de `prepararEntrada()`, o mapa `httpMethod` sozinho não produzirá 405 para GET.

## 5. Exercício de contribuição

Registre o corpo e status atuais, modifique uma única regra na sua cópia e compare os mesmos casos. Não altere ao mesmo tempo Router, Controller e HttpData para fazer um exemplo “passar”: você perderia a identificação de qual contrato mudou.

## Onde conferir no código

- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)
- [`galastri/modules/HttpData.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/HttpData.php)
- [`galastri/modules/Validation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Validation.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
