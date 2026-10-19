> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# VarDump — inspeção HTML durante o desenvolvimento

## Três entradas úteis

`VarDump::print(...$values)` apresenta valores em HTML com formatação, CSS, origem e linha de chamada. `VarDump::raw(...$values)` produz uma apresentação mais simples em `<pre>`, ainda com escape HTML.

Depois do bootstrap, o alias `\VarDump` e a função `vardump()` facilitam a chamada. O wrapper ajusta o nível do backtrace para apontar para o local de uso.

```php
vardump($dados, $resultado);
```

Use dentro de um laboratório HTML que permita esse conteúdo. O método não pausa a execução automaticamente; a requisição pode continuar depois de imprimir o diagnóstico.

## Como os valores são capturados

O método interno abre um buffer, chama `var_dump()` para cada valor, captura e escapa o texto, limpa para o próximo item e encerra o buffer em `finally`. A apresentação aplica regex para destacar tipos, chaves e valores.

Os métodos públicos `addBacktraceLevel()` e `getBacktraceLevel($reset)` dão suporte à origem dos wrappers. `RESET_BACKTRACE_LEVEL` e `KEEP_BACKTRACE_LEVEL` tornam o argumento legível.

## Canais estruturados

O resultado é HTML, inclusive em `raw()`. Inserir VarDump numa rota JSON ou num stream NDJSON acrescenta bytes fora do protocolo e pode impedir o parser do cliente de interpretar a resposta.

Para conferir um valor dessas rotas, reproduza o componente em `teste.php` ou faça um registro local apropriado. Também não use uma página pública de depuração para exibir credenciais ou dados reais da aplicação.

## Manutenção

Uma mudança nas regex deve ser verificada com valores nulos, booleanos, strings com tags, arrays aninhados e objetos. O objetivo é preservar o dado legível e o escape antes de melhorar a aparência. Uma formatação bonita que transforma texto em HTML executável quebra o contrato do diagnóstico.

## Onde conferir no código

- [`galastri/modules/VarDump.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/VarDump.php)
- [`galastri/bootstrap/functions.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/bootstrap/functions.php)
- [`galastri/bootstrap/bootstrap.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/bootstrap/bootstrap.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
