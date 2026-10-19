> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Tools — texto, arrays, importação e escrita de arquivos

## Grupos da API

| Métodos | Responsabilidade |
| --- | --- |
| `typeOf()` | Normaliza os nomes dos tipos PHP; pode retornar a classe de um objeto. |
| `toCamelCase()`, `toPascalCase()` | Convertem textos usados, por exemplo, em nomes de método e classe. |
| `readableImplode()` | Junta itens com separador final diferente. |
| `hasPlaceholders()`, `replacePlaceholders()` | Protocolo de marcadores detalhado no capítulo próprio. |
| `deleteFirstKey()`, `deleteLastKey()` | Retiram posições da sequência por slicing. |
| `arrayAt()` | Acessa uma posição, inclusive negativa, e pode remover o item. |
| `arrayFlatten()` | Achata valores de arrays aninhados, sem preservar a estrutura de chaves. |
| `import()` | Requer um arquivo relativo ao projeto ou devolve um padrão. |
| `trimLeft()`, `trimRight()`, `trimBoth()` | Removem alternativas literais nas bordas conforme a regex implementada. |
| `createDir()`, `chmod()`, `writeFile()` | Diretórios, permissões e escrita com bloqueio cooperativo. |

## Conversão de nomes

`toCamelCase()` troca separadores por espaços, aplica conversão de caixa multibyte, remove os espaços e coloca o primeiro caractere em minúscula. `toPascalCase()` coloca esse primeiro caractere em maiúscula.

Esses métodos participam do Router e da escolha de output. Uma alteração que pareça cosmética pode mudar a classe ou método procurado. Teste hífen, sublinhado, acento e nomes já compostos antes de trocar a regra.

## Arrays: posição não é necessariamente chave

`arrayAt($array, 0)` escolhe a primeira posição conforme `array_keys()`, mesmo se a chave real for textual. Um índice negativo conta a partir do fim. Com remoção ativada, usa `unset()` na chave encontrada; não promete reindexar o restante.

`deleteFirstKey()` e `deleteLastKey()` usam slicing, portanto observe as regras de preservação de chaves textuais e reindexação numérica. `arrayFlatten()` conserva valores de folhas, não uma associação de seus caminhos originais.

## Importação

`import($location, $defaultValue)` acrescenta o caminho a `PROJECT_DIR`, executa `require` quando o arquivo existe e devolve o retorno dele. Caso contrário, devolve o padrão informado.

O arquivo é código executável PHP, não um documento neutro de configuração. Não use um caminho livre recebido por HTTP para selecionar um arquivo a requerer.

## Escrita e permissões

```php
Tools::writeFile(
    PROJECT_DIR.'/var/exemplo.log',
    "Registro de demonstração\n",
    INSERT_CONTENT_AT_END,
    0600
);
```

Os modos são inserir no início, inserir no final e sobrescrever. O padrão do método é inserir no final. O código abre o arquivo em `c+b`, usa `flock(LOCK_EX)`, prepara o conteúdo, escreve tratando resultados parciais, descarrega e trunca para o tamanho final. `finally` fecha o handle.

A escrita de início ou final lê o conteúdo anterior para compor a nova string. Portanto, ela não é um append de custo constante para arquivos grandes. O bloqueio depende da cooperação dos demais escritores; não transforma uma falha de disco em atualização atômica com journal ou arquivo temporário.

As permissões são aplicadas ao arquivo novo. Um arquivo existente não recebe automaticamente o novo chmod informado. Os padrões dos utilitários são amplos; passe permissões deliberadas como `0600`, `0640` ou `0700` conforme o objeto e o usuário do serviço. A umask e as permissões dos diretórios também participam.

## Trim com alternativas

Os métodos de trim escapam as alternativas com `preg_quote()` e usam limites de palavra e comparação sem diferença de caixa no padrão. Eles não equivalem a `trim($texto, $mascaraDeCaracteres)` do PHP e podem ter comportamento particular com tokens terminados em pontuação.

## Ao manter Tools

Prefira testes pequenos por contrato. Para escrita, confira modos, arquivo pré-existente, conteúdo vazio e permissões. Para texto, confira literais que seriam metacaracteres regex. Para arrays, use chaves associativas e índices negativos.

Há mensagens técnicas ainda literais em alguns ramos de escrita no commit. Uma contribuição de centralização deve preservar a informação e incluir o catálogo Message, sem alterar silenciosamente o contrato da operação.

## Onde conferir no código

- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
