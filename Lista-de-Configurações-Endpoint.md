> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Configurações do contexto endpoint

Os valores abaixo vêm das definições desta edição. O arquivo de rotas da aplicação pode substituir parte deles durante o percurso.

| Nome | Tipo admitido | Valor padrão | Efeito ou limite |
| --- | --- | --- | --- |
| `method` | `string` | `""` | Substitui o método resolvido para o endpoint quando não vazio. |
| `parameters` | `string` | `""` | Declara segmentos obrigatórios e opcionais, por exemplo `/id/?aba`. |
| `httpMethod` | `array` | `[]` | Mapa de verbos HTTP para métodos adicionais do controller. |
| `viewFile` | `string` | `""` | Escolhe a view específica do endpoint; vazio usa a convenção. |

## O momento da aplicação

As propriedades são processadas depois que o endpoint foi selecionado. O método convencional é obtido do nome do endpoint e pode ser substituído por `method`. O mapa `httpMethod` é normalizado por verbo em minúsculas e tem `@` inicial removido dos valores.

## Exemplo

```php
'@salvar' => [
    'method' => 'prepararCadastro',
    'httpMethod' => [
        'POST' => '@receberCadastro',
    ],
    'parameters' => '/?origem',
],
```

O trecho pertence a um nó com `@main`. Numa requisição POST para esse endpoint, o construtor executa `prepararCadastro()` e depois `receberCadastro()`. Os dois devem retornar array. Numa requisição GET, o primeiro continua sendo executado e o mapa não acrescenta o segundo.

**O mapa não é uma lista automática de verbos permitidos.** Se a operação só pode aceitar POST, o controller ou uma alteração explícita de política precisa rejeitar os demais métodos.

## Parâmetros

`/id/?aba` exige `id` e torna `aba` opcional. Eles são segmentos da URL e continuam strings; não são argumentos posicionais do método PHP. Um segmento excedente invalida a rota das saídas que exigem controller.

## View específica

`viewFile` vazio deixa View procurar o arquivo convencional. Um valor específico continua relativo à pasta de views na montagem feita por View. A definição atual aceita nomes simples ou determinados caminhos terminados em `.php`; use a expressão real como referência antes de propor uma nova forma de caminho.

`httpMethod` só é validado como array pela Definition. Não há ali uma validação completa de cada chave e método. Ao evoluir esse contrato, teste valores malformados além dos casos corretos.

## Onde conferir no código

- [`galastri/core/config/definitions/endpoint.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/definitions/endpoint.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
