> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Placeholders — marcadores sequenciais, posicionais e nomeados

## Um utilitário compartilhado

`Tools::replacePlaceholders($message, $replacements = [])` substitui marcadores `{{...}}`. Ele é usado por mensagens, redirecionamentos e outros consumidores. `Tools::hasPlaceholders()` detecta marcadores não escapados, mas não substitui a validação completa realizada na troca.

## Três modos, um por template

| Modo | Template | Substituições | Resultado |
| --- | --- | --- | --- |
| Sequencial | `Olá, {{}}. Código {{}}.` | `['Ana', 42]` | `Olá, Ana. Código 42.` |
| Posicional, base 1 | `{{2}} / {{1}} / {{2}}` | `['A', 'B']` | `B / A / B` |
| Nomeado | `Olá, {{nome}}.` | `['nome' => 'Ana']` | `Olá, Ana.` |

Nos modos sequencial e posicional, os valores são obtidos de `array_values()`. No nomeado, a busca é pela chave. Não misture modos na mesma mensagem, mesmo que existam valores suficientes.

Um índice posicional começa em 1. Nomes seguem letras ASCII ou sublinhado no início, seguidos de letras, números ou sublinhado. Um marcador como `{{0}}` não é uma posição admitida.

## Escape literal

Uma barra invertida imediatamente anterior ao marcador o torna literal e é removida da saída. Em PHP com aspas simples:

```php
$result = Tools::replacePlaceholders('Literal: \\{{nome}}. Valor: {{nome}}.', ['nome' => 'Ana']);
```

Resultado: `Literal: {{nome}}. Valor: Ana.`. O marcador escapado não escolhe um modo de substituição para o restante da mensagem.

## Valores aceitos

Escalares, `null` e objetos que implementam `\Stringable` são convertidos para string. Arrays e objetos não convertíveis são rejeitados. A conversão segue PHP: um booleano falso ou nulo não vira automaticamente a palavra “não”.

Uma chave presente com nulo continua presente, pois a análise usa `array_key_exists()`. Um valor que contém outro marcador não é novamente tratado como template; ele é dado de substituição.

## Falhas e percentuais

Marcador inválido, modo misturado, posição inválida, valor ausente e valor incompatível produzem exceções próprias do catálogo. O erro de formatação precisa continuar tratável pelo próprio sistema de mensagens; por isso esse utilitário merece testes de falha, não só exemplos bem-sucedidos.

O caractere `%` não tem uma função especial nesse protocolo. Uma URL com `%20` permanece intacta, a menos que você modifique outro trecho da string explicitamente.

## O utilitário não faz escape contextual

Substituir valores não equivale a escapar HTML, codificar URL ou vincular parâmetros SQL. Use `$g->print()` para a apresentação HTML adequada, `rawurlencode()` para segmentos quando aplicável e binds PDO para valores SQL.

## Teste em teste.php

Depois de constantes e autoload:

```php
use galastri\modules\Tools;

$actual = Tools::replacePlaceholders('URL: /arquivo%20teste/{{id}}', ['id' => 42]);

if ($actual !== 'URL: /arquivo%20teste/42') {
    throw new \RuntimeException('A substituição alterou a URL.');
}

echo 'OK: placeholders sem conflito com percentuais.'.PHP_EOL;
```

Acrescente ao menos um teste de modo misturado e um de valor ausente ao alterar o parser.

## Onde conferir no código

- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)
- [`galastri/extensions/Exception.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/extensions/Exception.php)
- [`galastri/modules/Redirect.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Redirect.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
