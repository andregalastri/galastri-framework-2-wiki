> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Como evoluir o framework sem perder seus contratos

## Começar pela responsabilidade certa

| Mudança pretendida | Componentes a estudar |
| --- | --- |
| Novo endpoint de uma aplicação | Rotas e controller da aplicação. |
| Nova regra compartilhada de validação | Classe especializada, CoreValidation e mensagens. |
| Nova configuração | Definition, arquivo de definições, Router e consumidor. |
| Outra política de resposta | Galastri, classe de saída, ErrorHandler e clientes. |
| Outro driver de banco | Fábrica Database, driver, CoreDatabase e dialeto SQL. |
| Outra engine de template | View e contrato oferecido aos arquivos de apresentação. |

A tabela orienta uma contribuição futura. As funcionalidades novas descritas como exercício abaixo não estão implementadas no commit de referência.

## Exercício: uma regra de cor hexadecimal

Imagine acrescentar `isHexColor()` a `StringValidation`. A intenção é aceitar exatamente `#RRGGBB`, com seis dígitos hexadecimais, mantendo o modelo de registro adiado de regras.

Um método possível, **para estudar numa branch local**, seria inserido dentro da classe:

```php
public function isHexColor(): self
{
    $this->validationChain[] = function () {
        if (!is_string($this->value) || preg_match('/\A#[0-9a-fA-F]{6}\z/', $this->value) !== 1) {
            $this->messageFlagValues = [$this->value];
            $this->throwError();
        }
    };

    return $this;
}
```

Ele devolve o mesmo objeto e não verifica o dado no momento de montar a cadeia. Aproveita o mecanismo existente de falha e permite uma mensagem posterior por `onError()`.

Depois de implementar a proposta localmente, o teste correspondente poderia usar:

```php
$validator = Validation::string()
    ->isHexColor()
    ->onError('Informe uma cor no formato #RRGGBB.');

$validator->validate('#12aBcF');
```

Esse segundo trecho não funciona no commit original, porque o método é uma proposta. Um teste completo também precisa rejeitar `12aBcF`, `#abc`, `#12345G`, array e nulo, e comprovar a mensagem selecionada.

## Mensagens do núcleo

O método acima usa o mecanismo genérico existente. Se a contribuição exigir um novo texto técnico padrão, registre-o em Message e integre-o conscientemente sem fazer uma regra sobrescrever por acidente a mensagem personalizada de `onError()`.

Não resolva um problema de redação espalhando literais pelos métodos internos. Confira também se a nova mensagem pode ser formatada pelos placeholders sem provocar uma falha no próprio tratamento do erro.

## Nova configuração

O contrato em `definitions/*.php` é uma parte. O ponto de leitura no Router e o consumidor do valor são outras. Documente onde a configuração pode ser definida, o valor padrão, tipos, ausência e o que ocorre se ela mudar depois de o consumidor já ter tomado uma decisão.

A saída é um exemplo desse último caso: Galastri escolhe sua classe antes do controller. Um setter tardio em Config não troca automaticamente a referência guardada.

## Nova saída ou engine

A lista de outputs admitidos está na definição de `output`, e a escolha do nome da classe ocorre em Galastri. Uma saída precisa oferecer o contrato usado pelo núcleo, como `run()` e `requiresController()`, além de uma política de erro coerente.

View escolhe hoje PhpEngine por uma string interna. Não há uma chave de configuração de engine já pronta. Introduzir uma engine exige definir o contrato que templates recebem, como acesso a dados e escape, e revisar a compatibilidade com os arquivos existentes.

## Explicar impacto antes da implementação

Uma contribuição deve dizer qual entrada mudará de comportamento, que consumidor depende do retorno e quais casos próximos foram preservados. A menor mudança compreensível pode ser mais fácil de revisar que uma reformulação de vários subsistemas.

## Onde conferir no código

- [`galastri/modules/validation/StringValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/StringValidation.php)
- [`galastri/modules/validation/traits/CoreValidation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/CoreValidation.php)
- [`galastri/core/config/Definition.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Definition.php)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/core/output/View.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/View.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
