> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Laboratório JSON — construir um contrato explícito

## 1. Configurar uma rota JSON

Trecho dentro da raiz de `app/config/routes.php`:

```php
'/api' => [
    'output' => 'json',
    'includeProjectMetadata' => false,
    '@main' => [],
],
```

A raiz da aplicação ainda precisa de seu `@main`. O controller convencional desse nó será `app\controllers\Api`.

## 2. Criar o controller

Arquivo `app/controllers/Api.php`:

```php
<?php
namespace app\controllers;

use galastri\modules\Controller;

final class Api extends Controller
{
    protected function main(): array
    {
        return [
            'error' => false,
            'warning' => false,
            'code' => 'API_READY',
            'message' => 'API disponível.',
            'data' => ['version' => 1],
        ];
    }
}
```

Os cinco campos são uma decisão desse controller. Eles não são preenchidos por Json quando faltam.

## 3. Verificar o transporte

```bash
curl -i http://127.0.0.1:8080/api
```

Espere `Content-Type: application/json` e o objeto equivalente ao array. Unicode e barras não são escapados desnecessariamente. O JSON de sucesso é compacto, sem `JSON_PRETTY_PRINT`.

Para produzir um recurso com HTTP 201, o método precisa definir `http_response_code(201)` antes de retornar. Um campo `code` textual continua sendo código de aplicação.

## 4. Reunir retorno de endpoint e método HTTP

Um endpoint pode declarar `httpMethod => ['POST' => '@receber']`. O retorno do método de endpoint forma a base; o retorno de `receber()` é mesclado depois. Uma chave textual repetida fica com o valor do método HTTP.

Um mapa HTTP não impede por si uma requisição GET. Para uma ação de gravação, confira explicitamente o verbo no fluxo antes de fazer a operação. O laboratório HTTP integrado demonstra esse comportamento.

## Falhas antes e depois do streaming

Uma exceção anterior ao streaming pode produzir JSON de erro formatado com quebras de linha pelo ErrorHandler. Depois que o streaming começa, a apresentação de erro também precisa respeitar uma linha JSON por mensagem.

Um cliente que sempre tenta tratar toda resposta JSON como NDJSON pode quebrar no primeiro erro anterior ao streaming. Confira o `Content-Type` antes de escolher o parser.

## Metadados e cache

Com `includeProjectMetadata => true`, o retorno inclui `meta`. A política de autenticação é aplicada por Json, mas a implementação atual não chama a rotina de ETag usada por View e File. Definir `browserCache` não faz Json passar a emitir automaticamente o mesmo comportamento de 304 dessas saídas.

## Onde conferir no código

- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)
- [`galastri/core/ErrorHandler.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
