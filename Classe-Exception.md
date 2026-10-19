> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Exception — mensagens, códigos e dados adicionais

## Qual classe estamos usando

`galastri\extensions\Exception` estende `\Exception`. Seu construtor sem parâmetros formais interpreta `func_get_args()` para aceitar variações de mensagem, substituições, código e dados adicionais. Não é a assinatura padrão do construtor de `\Exception`.

## Formas de chamada

| Forma | Exemplo | Significado |
| --- | --- | --- |
| Texto | `new Exception('Falha de exemplo')` | Mensagem simples. |
| Texto e código | `new Exception('Falha de exemplo', 'EXEMPLO')` | Código de aplicação. |
| Texto e dados | `new Exception('Falha de exemplo', ['registro' => 42])` | Código padrão e dados adicionais. |
| Texto com placeholders | `new Exception('Registro {{}} inválido', [42], 'EXEMPLO')` | Substituições, depois código. |
| Par de catálogo | `new Exception(Message::get('VALIDATION_FAIL'))` | Mensagem e código vindos do catálogo. |
| Par com placeholders e dados | `new Exception(Message::get('CONFIG_DOESNT_EXIST'), ['nome'], ['origem' => 'laboratorio'])` | Substituições e dados; o código já está no par. |

Quando a mensagem vem num array, ele precisa conter ao menos mensagem e código. As formas com placeholders interpretam a posição seguinte como substituições; usar a ordem da exceção nativa nesse ponto produz outro comportamento.

## Códigos não são status HTTP

O código pode ser inteiro ou string nesta extensão. O ErrorHandler usa regras próprias para definir o status HTTP e preserva um erro HTTP já definido quando possível. Uma string `'404'` no construtor não é um contrato suficiente para produzir HTTP 404.

## Dados adicionais

`getData()` devolve o array completo. `getData('registro')` consulta uma chave ou retorna nulo quando ausente.

As chaves especiais `line` e `file`, quando fornecidas com valores aplicáveis, ajustam a origem da exceção e são retiradas do array restante. Esse comportamento atende diagnósticos internos; não aceite origem arbitrária de um usuário como se fosse a origem confiável de uma falha.

## Integração com a validação

No início da construção, a exceção copia `Validation::$displayError` para sua propriedade `displayError` e limpa a flag estática. Assim, uma mensagem de validação pode ser liberada sem deixar a mesma autorização pendente para outra exceção.

O sinal é consumido pela construção da exceção, não somente por uma captura posterior no ErrorHandler. Esse detalhe importa em testes que capturam a falha e continuam executando no mesmo request.

## Placeholders e erros de formatação

A extensão delega a análise e a substituição a Tools. Substituições não são novamente varridas como templates: um valor que contém `{{...}}` deve permanecer dado. O catálogo usa mensagens adequadas para os próprios erros de placeholders, evitando depender de `printf` e da sintaxe `%` de URLs.

Ao alterar o construtor, teste mensagem simples, par de catálogo, substituições, dados adicionais, código e uma formatação inválida. Isso verifica também os caminhos usados pelo próprio mecanismo de erro.

## Teste pequeno

Em `teste.php`, depois das constantes e autoload:

```php
$e = new \galastri\extensions\Exception(
    'Registro {{id}} inválido',
    ['id' => 42],
    'REGISTRO_INVALIDO',
    ['campo' => 'id']
);

echo $e->getMessage().PHP_EOL;
echo $e->getCode().PHP_EOL;
echo $e->getData('campo').PHP_EOL;
```

Espere `Registro 42 inválido`, `REGISTRO_INVALIDO` e `id`.

## Onde conferir no código

- [`galastri/extensions/Exception.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/extensions/Exception.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)
- [`galastri/language/Message.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/language/Message.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
