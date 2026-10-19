> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Glossário de manutenção

| Termo | Significado nesta documentação |
| --- | --- |
| Aplicação | Código hospedado em `app/`, com regras de negócio e apresentação próprias. |
| Núcleo | Coordenação, configuração, roteamento, saída e tratamento de erro do framework. |
| Bootstrap | Preparação que ocorre antes de Galastri atender a requisição. |
| Front controller | A entrada única `public/index.php` para o processamento encaminhado pelo servidor. |
| Raiz física | Pasta do projeto no disco, representada por `PROJECT_DIR`. |
| Raiz de URL | Prefixo configurado pela primeira chave externa de rotas, como `/` ou `/sistema`. |
| Nó | Parte da árvore de rotas identificada por chave `/nome`. |
| Nó dinâmico | Chave `/?nome` que captura um segmento da URL. |
| Endpoint | Chave `@nome` que seleciona uma operação no nó. |
| Parâmetro de caminho | Segmento declarado em `parameters`, depois da resolução do endpoint. |
| Query string | Dados depois de `?`; não são nós ou parâmetros de caminho. |
| Controller | Classe da aplicação estendida da base Controller; executa o método resolvido. |
| Método HTTP adicional | Método indicado no mapa `httpMethod` e executado depois do endpoint quando corresponde ao verbo. |
| Output | Classe selecionada para View, Json ou File. |
| View | Arquivo PHP de conteúdo ou a classe de saída, conforme o contexto. |
| Template | Arquivo de layout que pode incluir a view com PhpEngine. |
| Definition | Objeto que guarda o contrato e o valor de uma configuração. |
| Closure | Função que pode ser guardada como um valor e executada depois; usada nas filas de regras e queries. |
| PDO | Interface PHP de acesso a bancos, especializada por drivers como MySQL e SQLite. |
| DSN | String que descreve ao PDO o driver e o destino de uma conexão. |
| PSR-4 | Convenção de correspondência entre namespaces e caminhos de classes usada pelo autoload do projeto. |
| Superglobal | Variável PHP disponível no contexto da requisição, como `$_SERVER`, `$_GET` e `$_POST`. |
| Trait | Mecanismo PHP de compartilhar implementação entre classes. |
| Fábrica | Método que cria e retorna um objeto, como Database e Validation. |
| API fluente | Métodos que retornam o próprio objeto ou classe para permitir encadeamento. |
| Buffer de saída | Conteúdo temporariamente acumulado antes da transmissão ou captura. |
| NDJSON | Sequência de documentos JSON completos separados por nova linha. |
| Chunk | Porção arbitrária de bytes entregue pelo transporte; não necessariamente uma mensagem inteira. |
| ETag | Identificador de uma representação usado na validação de cache HTTP. |
| Tag de autenticação | Nome que identifica o estado e cookie próprios de uma autenticação. |
| Bind | Valor associado a um placeholder PDO de uma instrução SQL. |
| Submit | Momento em que CoreDatabase executa a fila de consultas. |
| Transação | Escopo de operações do banco confirmado ou desfeito conforme sua semântica. |
| SAPI | Forma de integração de PHP com seu ambiente, como CLI ou FPM. |
| WSL 2 | Ambiente Linux integrado ao Windows usado pelo caminho local deste curso. |
| SHA de referência | Identificador do commit que fundamenta as explicações técnicas. |

Use o glossário para alinhar nomes, mas confira a referência do componente para conhecer os tipos e efeitos concretos de cada operação.

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
