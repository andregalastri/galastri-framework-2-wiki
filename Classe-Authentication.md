> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Authentication — sessão, tag e cookie de token

## O que é autenticado

O módulo mantém uma autenticação por **tag**, combinando estado na sessão PHP e um cookie próprio com token. Ele não busca usuários num banco nem verifica senha, MFA ou identidade por conta própria. A aplicação deve validar as credenciais antes de criar essa autenticação.

O token é gerado com `random_bytes(48)` e codificado em base64. A tag escolhe as chaves usadas na sessão e no cookie; use um nome próprio da aplicação para evitar colisões no mesmo host.

## Ciclo de criação

```php
Authentication::setAuthTag('galastri_demo');
Authentication::setField('userId', 42);
Authentication::setCookieExpiration(3600);
Authentication::create();
```

O trecho assume credenciais já verificadas pela aplicação. `setAuthTag()` prepara ou recupera dados da tag; `setField()` e `setCookieExpiration()` ajustam o estado preparado. `create()` grava esse estado na sessão e emite o cookie.

Chamar apenas `setField()` não confirma a persistência na sessão. Depois de alterar os campos preparados, é `create()` que realiza a cópia para a autenticação persistida.

## Conteúdo e prazo

A sessão armazena token, IP, duração configurada, campos e `expiresAt`. O cookie próprio contém o token. A duração padrão preparada é 86400 segundos; `create()` calcula o prazo absoluto no momento da criação.

`validate()` confere o prazo no servidor, não somente a expiração que o navegador atribuiu ao cookie. Uma validação bem-sucedida não implementa renovação deslizante automática; outro `create()` é uma nova operação de persistência.

## Cookies e sessão PHP são peças diferentes

O cookie da tag é emitido com `Secure`, `HttpOnly` e `SameSite=Strict`. O código define domínio a partir de `HTTP_HOST` e usa caminho `/`. A infraestrutura deve validar o host recebido e oferecer HTTPS adequado ao cenário.

A sessão PHP possui também sua própria identificação e política de cookie. Configurar o cookie da tag não configura automaticamente todas as opções `session.cookie_*` do PHP. Ao preparar um ambiente HTTPS, confira ambos os contratos.

`setcookie()` envia uma instrução para a próxima interação do navegador; ele não preenche por si `$_COOKIE` na requisição atual. Teste a autenticação numa requisição seguinte usando os cookies efetivamente recebidos.

## Validação e campos

`validate($tag, $ipCheck = false)` exige sessão da tag, cookie textual com JSON contendo token, prazo inteiro válido e correspondência do token. A comparação de IP é opcional e desligada por padrão; o guard de Galastri chama a validação sem ativá-la.

`getField()` consulta a sessão, mas não refaz toda essa validação internamente. Use-o depois de validar a tag ou dentro de uma rota que passou pelo guard correspondente.

Campos admitem `bool|int|null|string`. Um array de permissões não pode ser passado diretamente a `setField()` com a assinatura atual; escolha uma representação ou carregue os dados da aplicação pelo identificador do usuário.

## API de remoção

`remove($tag)` remove a autenticação daquela tag, expira seu cookie e limpa o estado preparado correspondente. É a operação adequada quando a intenção é encerrar uma autenticação específica.

`destroy()` percorre todos os cookies recebidos e tenta expirá-los com os atributos usados pelo módulo, além de limpar e destruir a sessão e o estado estático. Seu alcance é maior; ele não equivale a remover só uma tag e não garante apagar cookies com atributos de domínio ou caminho diferentes.

## Registro e fechamento de sessão

O módulo inicia sessão quando necessário e chama `session_write_close()` nas rotinas de acesso. Ao alterar esse fluxo, confira concorrência, envio de cabeçalhos e estado após exceções. `create()` regenera o identificador da sessão por padrão e verifica o resultado dessa operação.

## Proteção de rota

Uma configuração `'authTag' => 'galastri_demo'` faz Galastri validar antes de instanciar o controller. A tag vazia não ativa essa exigência. A política de cache autenticado também usa a presença dessa configuração.

Veja o [laboratório de autenticação](/andregalastri/galastri-framework-2/wiki/Laboratorio-autenticacao) para observar as requisições e cookies sem misturar o exercício com um cadastro real de usuários.

## Onde conferir no código

- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/core/output/traits/BrowserCache.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/traits/BrowserCache.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
