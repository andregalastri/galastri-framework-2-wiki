> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Laboratório de autenticação — observar cookies e guard de rota

## Escopo do exercício

Este laboratório cria uma identidade fictícia para estudar o módulo, em servidor local restrito a loopback e com dados de demonstração. Um login real precisa validar credenciais e aplicar sua política de segurança antes de `Authentication::create()`.

Prepare o [HTTPS local](/andregalastri/galastri-framework-2/wiki/HTTPS-local) e mantenha os nomes de host e tag consistentes. As rotas abaixo são acrescentadas à raiz do arquivo de rotas da cópia de estudo.

## 1. Configurar rotas

```php
'/sessao-demo' => [
    'output' => 'json',
    'includeProjectMetadata' => false,
    '@main' => [],
],
'/painel' => [
    'output' => 'json',
    'includeProjectMetadata' => false,
    'authTag' => 'galastri_demo',
    '@main' => [],
],
```

## 2. Criar a identidade fictícia local

Arquivo `app/controllers/SessaoDemo.php`:

```php
<?php
namespace app\controllers;

use galastri\extensions\Exception;
use galastri\modules\Authentication;
use galastri\modules\Controller;

final class SessaoDemo extends Controller
{
    protected function main(): array
    {
        $localAddresses = ['127.0.0.1', '::1'];

        if (!in_array($_SERVER['REMOTE_ADDR'], $localAddresses, true)) {
            http_response_code(404);
            throw new Exception('Laboratório indisponível neste endereço.');
        }

        if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
            http_response_code(405);
            throw new Exception('Use POST neste laboratório.');
        }

        Authentication::setAuthTag('galastri_demo');
        Authentication::setField('userId', 42);
        Authentication::setCookieExpiration(300);
        Authentication::create();

        return ['message' => 'Identidade fictícia criada por cinco minutos.'];
    }
}
```

A checagem de endereço serve à limitação deste laboratório. Uma arquitetura com proxy precisa conferir o endereço efetivamente observado; não substitua o guard por confiança cega num cabeçalho fornecido pelo cliente.

## 3. Criar o destino protegido

Arquivo `app/controllers/Painel.php`:

```php
<?php
namespace app\controllers;

use galastri\modules\Authentication;
use galastri\modules\Controller;

final class Painel extends Controller
{
    protected function main(): array
    {
        return ['userId' => Authentication::getField('userId', 'galastri_demo')];
    }
}
```

Galastri verifica a tag antes de instanciar esse controller. O método consulta o campo depois do guard.

## 4. Fazer as requisições

Crie uma pasta local `var/` e use um arquivo de cookies descartável:

```bash
curl -i https://localhost:8443/painel
curl -i -c var/cookies-demo.txt -X POST https://localhost:8443/sessao-demo
curl -i -b var/cookies-demo.txt https://localhost:8443/painel
```

O certificado precisa estar confiável para o curl ou ser validado com `--cacert` apontando para o certificado público da sua autoridade local. Não é necessário desabilitar a validação TLS para estudar cookies.

Espere falha no primeiro acesso e `userId: 42` no último. A falha de autenticação do núcleo não define automaticamente 401/403 neste commit; observe a resposta real e compare com ErrorHandler.

O arquivo contém material de sessão: mantenha-o local e fora do Git. Ele não deve entrar na Wiki.

## 5. Encerrar e testar expiração

Para encerrar a tag específica num fluxo controlado, use `Authentication::remove('galastri_demo')` e verifique uma requisição seguinte. Para observar expiração, reduza a duração do laboratório e repita a consulta depois do prazo.

Não use `destroy()` para um logout de tag sem considerar seu alcance sobre sessão e outros cookies recebidos.

## O que este exercício demonstra

Ele permite observar o par sessão/cookie, o prazo no servidor, o guard de rota e o cache `private, no-store` de uma saída autenticada. Não implementa um cadastro real, armazenamento de senha ou sistema completo de permissões.

## Onde conferir no código

- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
