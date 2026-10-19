> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Base técnica e critérios desta edição

## Código de referência

- Repositório: [andregalastri/galastri-framework-2](https://github.com/andregalastri/galastri-framework-2).
- Branch consultada: `main`.
- Commit: [`55e91da329a50762a743f838c5899e60fc1e6764`](https://github.com/andregalastri/galastri-framework-2/commit/55e91da329a50762a743f838c5899e60fc1e6764).
- Mensagem: `fix: decodifica e valida caminho de arquivo na saída`.
- Data do commit: 19/10/2026 às 02:28:03, no horário de São Paulo.

Os links “Onde conferir no código” apontam para esse commit, para que uma atualização posterior da branch não troque silenciosamente a implementação que fundamenta uma explicação.

## Três tipos de informação

| Tipo | Como interpretar |
| --- | --- |
| Proposta registrada pelo autor | Objetivos recuperados do ZIP `Versão 0.1.1-alpha.md.zip`: simplicidade, aprendizado e organização próxima das URLs. |
| Comportamento observado no código | O que os métodos efetivamente fazem no commit de referência. |
| Orientação de manutenção | Uma forma recomendada de investigar ou modificar o projeto; não constitui uma API adicional. |

A Wiki anterior foi considerada somente para preservar o mecanismo de navegação lateral customizada e URLs úteis. O texto técnico anterior não é a fonte desta reescrita.

## Diferenças que merecem atenção

O `composer.json` atual declara PHP `>=8.0.0` e GD. Há uso efetivo de `mbstring`, Fileinfo, sessões e PDO nos componentes correspondentes. O guia de ambiente instala essas dependências de uso, mesmo quando não estão todas declaradas no Composer.

O módulo de progresso se chama `galastri\modules\ResponseStream`. Seu método `send()` encaminha um array a `Json::stream()`. Exemplos de versões antigas com outros nomes não devem ser copiados sem conferir a implementação.

O controller possui `getOutput()`, mas não um setter para trocar o formato de saída durante sua execução. O setter de download encontrado neste commit é `setDownloadableFile()`, e o getter é `isDownloadableFile()`.

## Limites da evidência

Ler um fluxo permite explicar sua estrutura; não comprova que toda combinação de servidor, versão do PHP e extensão funcione. Os capítulos distinguem exemplos executados, verificações estáticas e passos que precisam de validação no ambiente do colaborador. A página de validação registra os resultados desta edição.

## Como atualizar esta documentação depois

1. Registre o novo SHA e compare-o com esta referência.
2. Localize os contratos alterados: parâmetros, valores padrão, ordem de chamadas e efeitos colaterais.
3. Atualize o capítulo conceitual e a referência da classe correspondente.
4. Execute novamente os laboratórios afetados.
5. Atualize a evidência de validação e confira os links internos.

Atualizar somente o SHA sem revisar o texto produziria uma falsa garantia de atualidade.

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)
- [`galastri/modules/ResponseStream.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/ResponseStream.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
