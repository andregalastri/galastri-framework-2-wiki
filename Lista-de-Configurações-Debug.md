> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Configurações do contexto debug

Os valores abaixo vêm das definições desta edição. O arquivo de rotas da aplicação pode substituir parte deles durante o percurso.

| Nome | Tipo admitido | Valor padrão | Efeito ou limite |
| --- | --- | --- | --- |
| `displayErrors` | `bool` | `false` | Libera detalhes técnicos da mensagem, arquivo e linha na apresentação do erro. |
| `stopOnWarnings` | `bool` | `true` | Quando verdadeiro, avisos também encerram a execução depois da apresentação. |
| `showTrace` | `bool` | `false` | Exibe trace somente quando displayErrors também estiver ativo. |
| `performanceAnalysis` | `bool` | `false` | Definida, mas sem analisador de performance ativo no fluxo consultado. |
| `useErrorHandler` | `bool` | `true` | Definida, mas não desativa o registro incondicional do ErrorHandler no bootstrap. |
| `createLogsOnError` | `bool` | `true` | Controla a criação do log do framework depois da apresentação do erro. |

## Definição não equivale a recurso ativo

`displayErrors`, `stopOnWarnings`, `showTrace` e `createLogsOnError` são aplicadas pelo Router e consumidas pelo ErrorHandler. `performanceAnalysis` e `useErrorHandler` existem como definições, mas suas leituras no fluxo de roteamento estão comentadas e o bootstrap registra o handler independentemente da flag.

Por isso, definir `useErrorHandler => false` na árvore atual não implementa uma desativação geral do mecanismo. Não documente essa opção como interruptor funcional sem alterar e testar seu consumidor.

## Ambiente de estudo

```php
<?php
return [
    '/' => [
        'displayErrors' => true,
        'showTrace' => true,
        'createLogsOnError' => true,
        '@main' => [],
    ],
];
```

A aplicação inicial já usa `displayErrors => true` na raiz. Para um atendimento público, revise a política de exposição de detalhes. Logs preservam informação técnica mesmo quando a apresentação é genérica.

## Falhas antes de a rota carregar

O ErrorHandler consulta `Config::get('displayErrors', false)`. Antes de as definições ou a rota existirem, o fallback impede depender de uma configuração ausente. Porém, `public/index.php` ativa a exibição PHP antes do registro do handler; falhas muito iniciais ainda podem escapar desse tratamento.

Uma exceção de validação pode forçar a exibição de sua mensagem sem liberar arquivo, linha e trace. Essa regra é específica e está explicada nas referências de Validation e Exception.

## Avisos e continuidade

Com `stopOnWarnings => false`, determinados avisos são apresentados e a execução pode continuar. Isso não significa que JSON continue válido depois de um bloco de erro já impresso. Para respostas estruturadas, investigue a causa do aviso e observe o corpo completo, não apenas o status final.

## Onde conferir no código

- [`galastri/core/config/definitions/debug.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/definitions/debug.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
