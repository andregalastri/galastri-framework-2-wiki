> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Diagnóstico — localizar a camada antes de corrigir

## Uma sequência de investigação

Comece pela entrada e pare na primeira divergência observável. Um erro de socket não é um defeito de Router; um array correto no controller não garante que o template o procure na chave certa.

| Sintoma | Camada provável | Conferência inicial |
| --- | --- | --- |
| Conexão recusada | Processo e porta | Serviço ativo e endereço usado. |
| Apache 503 ou Nginx 502 | Integração FPM | Serviço, socket e permissões. |
| Arquivo PHP é baixado ou mostrado como texto | Handler de execução | Configuração do servidor; não continue expondo código. |
| Falta `vendor/autoload.php` | Preparação do projeto | `composer install` na raiz correta. |
| `mb_*` inexistente | Extensão PHP | `mbstring` na CLI ou FPM efetivamente usado. |
| Funciona em `/`, falha em outras URLs | Reescrita ou árvore de rotas | Encaminhamento ao index e resolução no Router. |
| Controller não encontrado | Namespace e arquivo | Caixa, caminho e classe declarada. |
| Método não encontrado | Endpoint e mapa HTTP | Método final resolvido e sua implementação. |
| View não encontrada | Convenção de apresentação | Nome do controller, método resolvido e `viewFile`. |
| Título ou versão ausentes | Estrutura de dados | Chave consultada no template e presença de `meta`. |
| JSON começa com HTML | Saída anterior ou diagnóstico | `echo`, VarDump, aviso PHP e buffer anterior. |
| Progresso aparece todo no final | Buffers e cliente | Descarga PHP/FastCGI/proxy e leitura por linha. |
| `could not find driver` | Extensão PDO | Driver da instalação PHP que atende o teste. |
| `Access denied` | Banco e credenciais | Conta, host de autenticação e privilégio do perfil. |
| Login não persiste | Sessão e cookies | HTTPS, host, prazo e cookie efetivamente aceito. |
| Não há log do framework | Permissões e configuração | Pasta gravável e `createLogsOnError`. |

## Separar CLI de FPM

`php -v`, `php --ini` e `php -m` descrevem a CLI que você chamou. O serviço FPM pode ser outra versão, carregar outro arquivo de configuração e executar com outro usuário.

Para investigar pelo HTTP, produza temporariamente uma resposta controlada com versão e extensões na cópia local. Remova o diagnóstico depois; uma página permanente de `phpinfo()` não faz parte do contrato da aplicação.

## Ler o erro sem esconder a causa

Se o ErrorHandler mostra uma mensagem genérica, consulte o log local ou ative detalhes somente no ambiente de estudo. A configuração de `showTrace` depende também de `displayErrors`; ativar apenas o trace pode não mostrar nada.

Uma falha ao criar o próprio log pede conferir permissões antes de mudar Exception. Se a resposta foi iniciada, o status HTTP pode não refletir a falha posterior; leia também o corpo e, no NDJSON, a mensagem final.

## Criar uma reprodução pequena

Reduza a rota e o controller, preserve a entrada que falha e remova dependências que não participam. Para uma regra de texto, talvez só `teste.php` com autoload seja necessário. Para reescrita, use o servidor real e curl.

Guarde commit, comando executado, resultado esperado, resultado real e versões. Um relato “não funcionou” não diferencia uma exceção esperada de falha na montagem do ambiente.

## Perguntas úteis para revisão

1. O caso passou pelo componente que eu estou alterando?
2. A entrada é admitida pelo contrato atual?
3. O teste falhou por causa do comportamento ou da preparação do cenário?
4. O resultado antigo estava sendo reaproveitado por estado residual?
5. O teste confirma também um caso vizinho que deve permanecer válido?

Essas perguntas evitam uma correção que apenas desloca o sintoma para a próxima camada.

## Onde conferir no código

- [`galastri/core/ErrorHandler.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
