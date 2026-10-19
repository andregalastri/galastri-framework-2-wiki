> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Message — catálogo central de mensagens do núcleo

## Retorno de get

`Message::get(string $messageConstName): array` retorna dois itens: o texto da constante privada e seu identificador. Exemplo conceitual:

```php
$message = Message::get('VALIDATION_FAIL');
```

O valor é equivalente a `['Os dados informados são inválidos.', 'VALIDATION_FAIL']`. A exceção personalizada entende esse par e preserva o identificador como código.

## Por que centralizar

A centralização permite revisar a redação, reconhecer falhas por um código estável e formatar valores sem espalhar textos técnicos pelos métodos. Nos caminhos do núcleo, prefira o catálogo a uma nova string literal lançada diretamente.

O diretório se chama `language`, mas esta classe não implementa seleção dinâmica de idioma, arquivos por localidade ou pluralização. Documente o catálogo existente sem atribuir a ele um sistema de internacionalização completo.

## Placeholders

Os textos usam `{{}}`, `{{1}}` ou `{{nome}}`, conforme as regras de Tools. No catálogo atual, a forma sequencial é frequente. O par retornado por Message não realiza a substituição; isso acontece quando o consumidor, como Exception, processa o texto.

## Como acrescentar uma mensagem

1. Escolha um identificador que descreva a falha e não dependa de um valor específico.
2. Acrescente a constante privada no grupo coerente do catálogo.
3. Use placeholders para valores variáveis e mantenha seu modo consistente.
4. Lance a exceção usando `Message::get('IDENTIFICADOR')` e as substituições na posição correta.
5. Confira o texto final e o código num `teste.php` com um caso representativo.

Esse procedimento é orientação para uma futura contribuição de código. Esta Wiki não acrescenta constantes ao repositório.

## Chave inexistente e limites atuais

`get()` acessa diretamente uma constante pelo nome. Não há fallback próprio para um identificador inexistente; PHP produz uma falha ao tentar resolver a constante. Trate erros de digitação no catálogo como defeito de implementação, não como uma mensagem de negócio esperada.

Nem todas as strings técnicas do commit já estão centralizadas: há ramos em Tools e Json com literais. Isso não muda o padrão desejado para novas contribuições; a página de limites registra esses pontos sem alterá-los.

## Onde conferir no código

- [`galastri/language/Message.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/language/Message.php)
- [`galastri/extensions/Exception.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/extensions/Exception.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
