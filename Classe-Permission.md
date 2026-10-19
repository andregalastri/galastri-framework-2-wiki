> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Permission — comparar permissões disponíveis e exigidas

## Autenticação e autorização têm entradas diferentes

Authentication verifica uma identidade persistida por tag. Permission compara conjuntos de permissões que a aplicação fornece. O módulo não descobre permissões no banco e não faz essa ligação automaticamente.

O construtor lê `permissionFailMessage` de Config. Em testes isolados, carregue Config antes de instanciá-lo.

## Qual conjunto vai em cada método

```php
$permission = new Permission();
$permission->checkFor(['clientes.ver', 'clientes.editar']);
$permission->at('clientes.editar');
$permission->validate();
```

`checkFor()` recebe as permissões disponíveis ao usuário. `at()` recebe as exigidas pela operação. Ambos aceitam strings e arrays variádicos, que são achatados por Tools.

## Uma ou todas

O padrão é aceitar qualquer correspondência, equivalente ao estado configurado por `allowAny()`. `requireAll()` exige que nenhuma permissão esperada esteja ausente das disponíveis.

```php
$permission->at('clientes.ver', 'clientes.editar')
    ->requireAll()
    ->validate();
```

As operações de comparação são `array_diff()` e `array_intersect()`, com a semântica dessas funções. Use identificadores textuais consistentes; não apresente a implementação como um sistema de tipos ou hierarquia automática de papéis.

Uma lista esperada vazia passa no modo “todas”, porque não há item faltante, e falha no modo “qualquer”, porque não há correspondência. Escolha conscientemente se uma operação sem exigências deve chegar a esse módulo.

## Mensagem e falha

`setFailMessage($message, $replacements = [])` aplica placeholders e troca a mensagem da instância. Uma falha lança Exception com código `PERMISSION_DENIED`.

O módulo não define automaticamente HTTP 403 nem libera a mensagem como uma validação de formulário. Se a API precisa de 403, o fluxo da aplicação deve controlar esse status no momento apropriado.

## Estado e reset

`validate()` não limpa a configuração. `reset()` apaga conjuntos, volta ao modo “qualquer” e restaura a mensagem padrão capturada no construtor.

Ao usar o mesmo objeto para várias verificações, chame reset ou substitua deliberadamente toda configuração. Uma permissão esperada da operação anterior não deve participar por acidente da seguinte.

## Teste útil

Monte duas permissões disponíveis e compare uma exigência satisfeita, uma ausente, a combinação “qualquer”, a combinação “todas” e uma lista vazia. Capture a exceção e confira o código, além da decisão de permitir ou rejeitar.

Uma checagem que apenas esconde um botão no frontend não substitui a execução da regra no backend antes da operação protegida.

## Onde conferir no código

- [`galastri/modules/Permission.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Permission.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
