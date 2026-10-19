> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Ambiente local — qual caminho seguir

## Um ambiente Linux para os dois sistemas

O caminho de referência é **Ubuntu 24.04 LTS com PHP 8.3 dos repositórios da distribuição**. É uma base reproduzível para o curso, não uma alteração do requisito mínimo do framework e nem uma afirmação de que esta seja a única combinação utilizável.

| Seu computador | Caminho do curso |
| --- | --- |
| Linux com Ubuntu 24.04 | Instale as ferramentas no próprio Linux. |
| Windows | Instale Ubuntu 24.04 no WSL 2 e siga os mesmos comandos Linux. |
| Outra distribuição ou outra versão PHP 8 suportada | Adapte os nomes dos pacotes e registre sua combinação ao reportar um problema. |

O `composer.json` exige PHP `>=8.0.0`. Não interprete isso como recomendação de instalar uma versão fora de suporte. Para reproduzir um defeito específico em PHP 8.5, mantenha um ambiente separado com a mesma versão e as extensões correspondentes.

## Componentes necessários

| Componente | Uso no projeto |
| --- | --- |
| PHP CLI | Rodar `teste.php`, verificar sintaxe e usar o servidor local. |
| Composer | Gerar `vendor/autoload.php` de acordo com o projeto. |
| Git | Obter o código e comparar alterações. |
| `mbstring` | Conversões de nomes, validação, download e outras operações de texto. |
| GD com suporte a FreeType | Resposta gráfica de erros na saída File. |
| Fileinfo | `mime_content_type()` na entrega de arquivos. |
| Sessões PHP | `Authentication`. |
| PDO e `pdo_sqlite` | Laboratórios de banco sem serviço externo. |
| `pdo_mysql` | Experiências que usam MySQL ou MariaDB. |
| Apache ou Nginx com PHP-FPM | Conferir o comportamento real de reescrita e integração HTTP. |

A aplicação inicial não precisa de um servidor MySQL para abrir. SQLite permite estudar o módulo Database antes de acrescentar outro serviço.

## Sequência prática

1. Se estiver no Windows, faça primeiro a preparação WSL 2.
2. Instale os pacotes no Ubuntu e clone o projeto.
3. Instale o autoload com Composer.
4. Rode a verificação de extensões e abra a aplicação com o servidor embutido.
5. Passe para Apache ou Nginx quando o teste depender das regras do servidor ou de streaming.

O servidor embutido é uma ferramenta de desenvolvimento. Ele não lê `.htaccess`; o comando do curso informa explicitamente o arquivo de entrada como roteador.

## Windows nativo e Docker

Executar PHP nativo no Windows pode ser possível, mas cria outro conjunto de caminhos, permissões, extensões e regras de servidor para verificar. O curso adota WSL 2 para manter a referência Linux e simplificar a reprodução entre colaboradores.

Docker também pode padronizar versões. Este commit não fornece uma composição Docker de desenvolvimento; portanto não há um comando `docker compose up` do projeto a ser ensinado como recurso pronto. Uma composição futura deve declarar versões, extensões, volume persistente e usuário de execução antes de virar caminho oficial.

## Referências externas

- [Instalação do WSL — Microsoft](https://learn.microsoft.com/en-us/windows/wsl/install).
- [Arquivos no ambiente WSL — Microsoft](https://learn.microsoft.com/en-us/windows/wsl/setup/environment).
- [PHP 8.3 CLI no Ubuntu 24.04 — Canonical](https://packages.ubuntu.com/noble/php8.3-cli).
- [Uso básico do Composer](https://getcomposer.org/doc/01-basic-usage.md).
- [Servidor embutido do PHP](https://www.php.net/manual/en/features.commandline.webserver.php).

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)
- [`galastri/core/output/File.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/File.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
