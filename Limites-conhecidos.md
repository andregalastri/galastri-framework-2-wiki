> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Limites observados no commit de referência

## Como usar esta página

Esta é uma referência para manter expectativas corretas durante o curso. Os itens descrevem o código analisado; não são funcionalidades acrescentadas pela Wiki nem correções aplicadas ao repositório.

| Área | Comportamento observado | Implicação para manutenção |
| --- | --- | --- |
| Mapa `httpMethod` | Acrescenta um método para o verbo correspondente. | Não é uma lista automática de verbos permitidos e não gera 405 sozinho. |
| Controller | Chama o endpoint resolvido e, quando houver, o método HTTP. | `main()` não é um hook universal. Os hooks comentados não estão ativos. |
| Sucesso JSON | Serializa o array da aplicação. | O envelope de negócio não é preenchido automaticamente. |
| Configuração | Contratos e pontos de leitura são separados. | Uma Definition nova não passa a ser lida automaticamente no Router. |
| Debug | `useErrorHandler` e `performanceAnalysis` estão definidos sem o fluxo funcional sugerido pelos nomes. | Não prometer um interruptor de handler ou analisador de performance pronto. |
| Bootstrap | Ativa exibição PHP antes de registrar o handler. | Falhas muito iniciais podem escapar da apresentação configurada pela rota. |
| ErrorHandler | Não desfaz genericamente toda saída anterior. | Texto já impresso pode contaminar JSON e não pode ser recolhido depois do envio. |
| Erro File em PHP 8.5 | O ramo gráfico ainda chama `imagedestroy()`, depreciado nessa versão. | Avisos podem contaminar a resposta de imagem; verificar com GD/FreeType no ambiente nativo. |
| Streaming | A flag é estática e o protocolo usa linhas. | Não há reset geral para workers persistentes; chunk não equivale a mensagem. |
| Streaming: ramo excepcional | Json usa `new Exception` sem importação dessa classe no arquivo. | Um buffer não removível pode produzir erro de classe ausente em vez da exceção pretendida. |
| Cache | View e File calculam a resposta antes do ETag; Json não chama essa comparação. | 304 não elimina automaticamente o trabalho do controller ou de renderização. |
| Arquivos | Leitura integral em memória; caminho do controller pula a validação física. | Não há download por blocos ou proteção universal de MIME para todo conteúdo gerado. |
| Banco | Fila e binds pertencem ao objeto; submit não limpa binds. | Submeter cada conjunto ou administrar parâmetros conscientemente. |
| Transações | Segundo begin é recusado. | Não há savepoints, nesting ou marcação automática de rollback obrigatório. |
| Validação | Cadeia inversa e fábricas sem regra automática de tipo. | A ordem visual não é a ordem de execução; cadeia vazia não valida o formato. |
| `restrictList` | Assinatura variádica `int|string`. | Array precisa ser desempacotado, embora a implementação use flatten internamente. |
| Texto | Conjuntos literais são escapados; flags possuem expansões específicas. | `a-z` literal não é faixa, e `--numbersUtf8` não representa todos os dígitos Unicode. |
| CNPJ | A implementação admite os formatos numéricos previstos. | Um formato alfanumérico não é atendido por esse método. |
| Autenticação | Cookie próprio e sessão precisam concordar, com prazo no servidor. | Sessão criada não comprova cookie aceito pelo navegador; não há validação de credencial no módulo. |
| HTTP de falhas | Códigos de domínio não são automaticamente 401/403/422. | A aplicação precisa definir seu contrato de status explicitamente. |
| Mensagens | Alguns ramos de Json e Tools ainda usam literais. | A centralização completa não deve ser descrita como concluída. |
| Aplicação inicial | Template/view consultam algumas chaves que o controller inicial não fornece diretamente. | Um título ou versão em branco pode ser uma divergência do exemplo, sem falha geral de renderização. |
| Testes e ambiente | Não há composição Docker ou pipeline de testes declarada no Composer. | Os procedimentos desta Wiki são laboratórios documentados, não scripts já versionados no núcleo. |

## Separar melhoria proposta de contrato atual

Uma evolução pode mudar qualquer um desses pontos, mas exige implementação, testes e atualização da documentação. Não “corrija” a explicação dizendo que uma função deveria fazer algo que ainda não faz.

Para cada proposta, registre o gatilho, o comportamento atual, o desejado, os consumidores afetados e a reprodução em `teste.php` ou HTTP. As páginas de referência vinculadas pelo menu oferecem o contexto para decidir o alcance da mudança.

## Sobre versões PHP

Uma ocorrência em PHP 8.5 não deve ser atribuída automaticamente à versão. Verifique primeiro o erro concreto, extensões, SAPI e contrato executado. A mesma falha de configuração pode ocorrer em várias versões, enquanto uma incompatibilidade real precisa ser reproduzida e delimitada.

## Onde conferir no código

- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/modules/database/traits/CoreDatabase.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/traits/CoreDatabase.php)
- [`galastri/modules/validation/StringValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/StringValidation.php)
- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)
- [`app/templates/template.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/templates/template.php)
- [`app/views/Index/main.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/app/views/Index/main.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
