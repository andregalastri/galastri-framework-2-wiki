> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Estrutura de pastas e mapa de responsabilidades

## Comece pela fronteira entre aplicação e framework

`app/` contém a aplicação hospedada pelo framework. `galastri/` contém o framework. `public/` é a raiz que o servidor web deve expor. Essa separação permite trabalhar no núcleo e usar uma aplicação pequena para observar o efeito da mudança.

| Caminho | O que existe nesta edição | Papel na manutenção |
| --- | --- | --- |
| `public/index.php` | Entrada PHP | Define a política inicial de erros e inclui o bootstrap. |
| `public/.htaccess` | Regras do Apache | Encaminha requisições e trata favicon, diretórios e compressão. |
| `public/nginx.conf` | Fragmento de configuração | Explica regras de encaminhamento; precisa de um bloco PHP-FPM no servidor. |
| `galastri/bootstrap/` | Bootstrap, constantes e função auxiliar | Prepara autoload, buffers e ErrorHandler. |
| `galastri/core/` | `Galastri`, `Router`, `ErrorHandler` | Orquestração, resolução de rotas e apresentação de erros. |
| `galastri/core/config/` | `Config`, `Definition`, definições | Contratos e valores de configuração. |
| `galastri/core/output/` | Saídas, engine e cache | Última etapa de formação da resposta. |
| `galastri/modules/` | Ferramentas de aplicação | Entrada HTTP, controllers, banco, autenticação, validação e outros. |
| `galastri/extensions/` | Exceção especializada | Integra mensagens, placeholders e dados de diagnóstico. |
| `galastri/language/Message.php` | Catálogo de mensagens | Texto e identificador das falhas. |
| `galastri/misc/` | Fonte, imagens e CSS internos | Recursos usados por diagnóstico e VarDump. |
| `app/config/routes.php` | Árvore da aplicação | Reproduções de roteamento e escolha de saída. |
| `app/config/database.php` | Perfis de conexão | Dados dos drivers; os exemplos fornecidos vêm vazios. |
| `app/config/mime-types.php` | Mapeamento de MIME | Validação de arquivos lidos do disco. |
| `app/controllers/Index.php` | Controller inicial | Menor ponto para seguir uma requisição completa. |
| `app/templates/`, `app/views/` | Layout e conteúdo | Laboratório da saída View. |
| `logs/` | Destino de relatórios | Deve ser gravável pelo usuário que executa PHP. |
| `vendor/` | Autoload do Composer | Gerado localmente; não deve ser reimplementado em uma correção comum. |
| `composer.json`, `composer.lock` | Dependências e autoload | Conferir juntos ao mudar requisitos. |

## Arquivos opcionais

O bootstrap inclui `app/bootstrap.php` somente se existir. `Config::run()` também procura definições adicionais em `app/config/definitions/*.php`. Essas possibilidades são verificações do código, não pastas obrigatoriamente preenchidas na aplicação inicial.

## Namespace e caminho

O Composer mapeia `galastri\` para `galastri/` e `app\` para `app/`. Assim, `app\controllers\Clientes` corresponde a `app/controllers/Clientes.php`. A classe do arquivo deve se chamar `Clientes` e declarar `namespace app\controllers;`.

Em Linux, maiúsculas e minúsculas do nome do arquivo importam. `clientes.php` não deve ser usado para uma classe carregada como `Clientes`. Trabalhar pelo WSL reduz a chance de uma divergência de caixa passar despercebida num ambiente Windows.

## Por que a raiz pública é somente public

A configuração de banco, logs, arquivos de dependências e código interno ficam fora da raiz pública. Expor a pasta inteira do projeto mudaria essa fronteira. A existência de roteamento PHP não substitui a configuração correta do `DocumentRoot` ou de `root` no servidor.

## Exercício de localização

Uma página mostra dados corretos, mas o título está vazio. A investigação pode passar por `routes.php`, `Controller::mergeReturns()` e o acesso `meta.title` na engine. Começar por `Database` não ajuda a explicar esse sintoma, porque a formação do título não depende de uma query.

Use a estrutura para localizar responsabilidades antes de procurar uma linha específica.

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`galastri/bootstrap/bootstrap.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/bootstrap/bootstrap.php)
- [`galastri/core/config/Config.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Config.php)
- [`app/controllers/Index.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/controllers/Index.php)
- [`public/.htaccess`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/public/.htaccess)
- [`public/nginx.conf`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/public/nginx.conf)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
