> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Roteiro de estudo — do ambiente à manutenção

O curso segue a ordem em que as dúvidas costumam aparecer para um colaborador: primeiro fazer funcionar, depois acompanhar uma requisição, depois alterar uma responsabilidade com conhecimento dos efeitos.

## Etapa 1 — Preparar e reconhecer

Leia [Ambiente local](/andregalastri/galastri-framework-2/wiki/Ambiente-local), [Visão geral](/andregalastri/galastri-framework-2/wiki/Vis%C3%A3o-Geral) e [Estrutura de pastas](/andregalastri/galastri-framework-2/wiki/Estrutura-de-pastas). Ao terminar, você deve conseguir abrir a aplicação de exemplo, explicar por que o servidor aponta para `public` e diferenciar a aplicação do núcleo.

**Verificação:** localize `public/index.php`, o bootstrap, a configuração de rotas, o controller inicial e a view que ele utiliza.

## Etapa 2 — Seguir uma requisição

Estude inicialização, bootstrap, `Galastri`, `Router`, `Config`, `Definition` e `Controller`. Não tente memorizar todos os métodos. Acompanhe uma URL específica e registre quais valores mudam a cada etapa.

**Verificação:** explique a diferença entre o nome do endpoint na URL, o método PHP resolvido e o método adicional associado ao verbo HTTP.

## Etapa 3 — Entender os contratos de saída

Compare View, JSON, arquivo, cache e streaming. O mesmo array do controller não passa por todos esses componentes; a classe de saída escolhida define o que será consumido.

**Verificação:** crie uma resposta JSON, uma página que use `$g->print()` e um arquivo produzido pelo controller. Mostre onde cada caminho se separa.

## Etapa 4 — Estudar os módulos com estado

Passe por entrada HTTP, validação, banco de dados, autenticação e permissões. Observe quais objetos podem ser instanciados e quais classes mantêm propriedades estáticas durante a requisição.

**Verificação:** explique a ordem inversa da cadeia de validação, a execução adiada de queries até `submit()` e a necessidade de chamar `create()` depois de configurar uma autenticação.

## Etapa 5 — Praticar manutenção

Use os laboratórios, a página de diagnóstico e o roteiro de contribuição. Um bom exercício é reproduzir um caso pequeno em `teste.php`, localizar o método responsável, formular uma alteração limitada e comparar os resultados antes e depois.

**Verificação:** descreva um contrato preservado, um caso corrigido e um teste que falharia antes da mudança.

## Índice para acompanhar a trilha completa

| Etapa | Leitura encadeada |
| --- | --- |
| Ambiente | [Windows/WSL 2](/andregalastri/galastri-framework-2/wiki/Ambiente-Windows-WSL2) → [Ubuntu](/andregalastri/galastri-framework-2/wiki/Ambiente-Ubuntu) → [Apache](/andregalastri/galastri-framework-2/wiki/Ambiente-Apache) ou [Nginx](/andregalastri/galastri-framework-2/wiki/Ambiente-Nginx) → [HTTPS](/andregalastri/galastri-framework-2/wiki/HTTPS-local). |
| Requisição | [Inicialização](/andregalastri/galastri-framework-2/wiki/Inicializacao) → [Bootstrap](/andregalastri/galastri-framework-2/wiki/Bootstrap) → [Galastri](/andregalastri/galastri-framework-2/wiki/Classe-Galastri) → [Router](/andregalastri/galastri-framework-2/wiki/Classe-Router) → [Controller](/andregalastri/galastri-framework-2/wiki/Classe-Controller). |
| Rotas e configuração | [Nós e endpoints](/andregalastri/galastri-framework-2/wiki/Vis%C3%A3o-Geral-Rotas) → [Dinâmicos e namespaces](/andregalastri/galastri-framework-2/wiki/Rotas-dinamicas-e-namespaces) → [Config](/andregalastri/galastri-framework-2/wiki/Classe-Config) → [Definition](/andregalastri/galastri-framework-2/wiki/Classe-Definition) → [Definição adicional](/andregalastri/galastri-framework-2/wiki/Arquivos-de-defini%C3%A7%C3%A3o-de-configura%C3%A7%C3%A3o). |
| Respostas | [Fluxo de dados](/andregalastri/galastri-framework-2/wiki/Vis%C3%A3o-Geral-Fluxo-de-dados) → [View](/andregalastri/galastri-framework-2/wiki/Formato-de-sa%C3%ADda-View) → [JSON](/andregalastri/galastri-framework-2/wiki/Formato-de-sa%C3%ADda-Json) → [NDJSON](/andregalastri/galastri-framework-2/wiki/Classe-ResponseStream) → [File](/andregalastri/galastri-framework-2/wiki/Formato-de-sa%C3%ADda-File) → [Cache](/andregalastri/galastri-framework-2/wiki/Trait-BrowserCache). |
| Falhas | [ErrorHandler](/andregalastri/galastri-framework-2/wiki/Classe-ErrorHandler) → [Exception](/andregalastri/galastri-framework-2/wiki/Classe-Exception) → [Message](/andregalastri/galastri-framework-2/wiki/Classe-Message). |
| Persistência | [Database](/andregalastri/galastri-framework-2/wiki/Classe-Database) → [SQLite](/andregalastri/galastri-framework-2/wiki/Classe-SqliteDatabase) → [MySQL](/andregalastri/galastri-framework-2/wiki/Classe-MysqlDatabase) → [CoreDatabase](/andregalastri/galastri-framework-2/wiki/Trait-CoreDatabase) → [Transações](/andregalastri/galastri-framework-2/wiki/Banco-transacoes). |
| Entrada | [HttpData](/andregalastri/galastri-framework-2/wiki/Classe-HttpData) → [Validation](/andregalastri/galastri-framework-2/wiki/Classe-Validation) → [Ordem das regras](/andregalastri/galastri-framework-2/wiki/Trait-CoreValidation) → [Texto](/andregalastri/galastri-framework-2/wiki/Classe-StringValidation), [Número](/andregalastri/galastri-framework-2/wiki/Classe-NumericValidation) e [Data](/andregalastri/galastri-framework-2/wiki/Classe-DateTimeValidation). |
| Identidade e autorização | [Authentication](/andregalastri/galastri-framework-2/wiki/Classe-Authentication) → [Laboratório](/andregalastri/galastri-framework-2/wiki/Laboratorio-autenticacao) → [Permission](/andregalastri/galastri-framework-2/wiki/Classe-Permission). |
| Utilitários | [Redirect](/andregalastri/galastri-framework-2/wiki/Classe-Redirect) → [Placeholders](/andregalastri/galastri-framework-2/wiki/Placeholders) → [Tools](/andregalastri/galastri-framework-2/wiki/Classe-Tools) → [VarDump](/andregalastri/galastri-framework-2/wiki/Classe-VarDump). |
| Prática | [HTTP integrado](/andregalastri/galastri-framework-2/wiki/Laboratorio-HTTP) → [teste.php](/andregalastri/galastri-framework-2/wiki/Laboratorio-testes) → [Como estender](/andregalastri/galastri-framework-2/wiki/Como-estender) → [Diagnóstico](/andregalastri/galastri-framework-2/wiki/Diagnostico) → [Contribuição](/andregalastri/galastri-framework-2/wiki/Contribuicao). |
| Consulta | [Glossário](/andregalastri/galastri-framework-2/wiki/Glossario), [Índice da API](/andregalastri/galastri-framework-2/wiki/Indice-da-API), [Limites conhecidos](/andregalastri/galastri-framework-2/wiki/Limites-conhecidos) e [Validação desta edição](/andregalastri/galastri-framework-2/wiki/Validacao-desta-edicao). |

## Como aproveitar a referência de classes

Os capítulos de classe são consultas de apoio ao curso. Procure a referência quando precisar saber se um método retorna um array, modifica estado, encerra a execução ou depende de configuração já carregada.

Ao estudar um método, pergunte também o que ele **não verifica**. Por exemplo: um parâmetro de rota pode estar presente e ainda conter um valor inválido para o negócio; uma resposta JSON pode estar bem formada e ainda carregar dados que a aplicação não deveria divulgar.

## Exercícios sem respostas decoradas

1. Por que incluir apenas `vendor/autoload.php` é diferente de incluir o bootstrap completo?
2. Uma rota configurada com `httpMethod` rejeita automaticamente todos os demais verbos?
3. O retorno de `main()` sempre participa de uma chamada a outro endpoint?
4. Um `onError()` de validação vale para regras colocadas antes ou depois dele?
5. Enviar `flush()` garante que o navegador receba um pacote naquele instante?

As respostas devem vir do fluxo do código e das experiências, não de suposições sobre outros frameworks.

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
