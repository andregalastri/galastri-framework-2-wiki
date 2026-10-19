> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# PhpEngine — acessar e apresentar os dados

## A instância $g

View cria `galastri\core\output\engines\PhpEngine` antes de incluir template ou view. O construtor copia para sua propriedade o array retornado por `Galastri::getControllerResponse()`.

## get: navegação por chaves

```php
$nome = $g->get('data', 'cliente', 'nome');
$primeiro = $g->get('data', 'clientes', 0, 'nome');
```

Os argumentos são chaves sucessivas, não uma expressão de caminho com pontos. Se um nível não for array ou uma chave não existir, `get()` retorna `null`. Se a chave existir com `false`, `0`, `'0'` ou `[]`, esse valor é preservado.

Sem argumentos, o método retorna o array completo do controller. `get()` não escapa o dado: isso permite fazer laços e cálculos, mas exige escolher a codificação adequada quando você o imprime.

## print: saída escalar com escape HTML

```php
<?php $g->print('data', 'cliente', 'nome'); ?>
```

O método consulta `get()`, converte array vazio em nulo e transforma nulo em string vazia. Strings e números são impressos com `htmlspecialchars(..., ENT_QUOTES, 'UTF-8')`.

Booleanos, arrays não vazios, objetos e recursos geram exceção. Um booleano deve ser apresentado deliberadamente pelo template:

```php
<?php
$ativo = $g->get('data', 'cliente', 'ativo');

if ($ativo) {
    echo 'Ativo';
} else {
    echo 'Inativo';
}
?>
```

Isso evita adivinhar se `false` deveria virar vazio, “não”, zero ou outro texto de negócio.

## include: view ou arquivo do projeto

`$g->include('view')` inclui o caminho que View já resolveu. Outro caminho é interpretado a partir de `PROJECT_DIR`, por exemplo `$g->include('/app/templates/partes/menu.php')`.

A variável `$g` é preservada dentro do arquivo incluído. Um caminho inexistente retorna `null`. O retorno declarado é `?bool`; arquivos de apresentação não devem devolver arrays ou outros dados incompatíveis com esse contrato.

O método executa PHP e não é um carregador seguro para qualquer caminho recebido por HTTP. Selecionar arquivos de apresentação a partir de entrada livre ampliaria a responsabilidade e os riscos do módulo.

## Um teste de regressão útil

Prepare dados com `false`, `0`, `''`, `[]`, uma chave ausente e uma chave interna ausente. Confira `get()` para cada caso e `print()` para os tipos admitidos. O sucesso de uma string comum não verifica a diferença entre “chave ausente” e “valor falso”.

Ao mudar a forma de navegação, preserve o acesso por índices numéricos e o retorno de valores falsy existentes.

## Onde conferir no código

- [`galastri/core/output/engines/PhpEngine.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/engines/PhpEngine.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
