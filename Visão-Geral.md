> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Visão geral e filosofia do framework

## De estudo de PHP a ferramenta para aplicações

O autor apresenta o Galastri como um microframework PHP iniciado como estudo da linguagem e desenvolvido para organizar sites, sistemas e APIs. Um objetivo registrado é facilitar a entrada em programação PHP e, ao mesmo tempo, permitir projetos reais.

Para o mantenedor, isso tem uma consequência prática: uma mudança precisa ser compreensível. Não basta reduzir linhas ou adotar um padrão conhecido; é preciso mostrar onde o comportamento nasce, qual problema resolve e como o colaborador consegue depurá-lo.

## Organização próxima da URL

O projeto representa a navegação como uma árvore de **nós** e **endpoints**. Um nó costuma levar a uma classe de controller; um endpoint costuma levar a um método dessa classe. A correspondência pode ser ajustada por configuração, portanto não é uma equivalência rígida entre cada texto da URL e um arquivo físico.

Considere `/clientes/detalhes/42`. Em um exemplo configurado com `/clientes`, `@detalhes` e `parameters => '/id'`, o Router pode resolver a classe `app\controllers\Clientes`, o método `detalhes()` e o parâmetro textual `42`.

O valor `42` não vira argumento posicional do método. O controller o consulta por `getParameter('id')`. Esse detalhe conecta a filosofia da URL ao contrato efetivo do código.

## A descrição Thin Model MVC

A documentação original usa a expressão **Thin Model MVC** para explicar que controllers e views possuem convenções próprias, enquanto a organização de models fica a cargo da aplicação.

No código atual, isso se traduz em uma base `Controller`, classes específicas para renderização e módulos utilitários. Não há um ORM, uma classe Model obrigatória, migrations automáticas ou uma estrutura de domínio imposta pelo núcleo. Um mantenedor não deve atribuir ao framework essas funções porque existem em outro produto com arquitetura MVC.

A palavra “view” também aparece em dois sentidos. Pode designar o arquivo PHP da interface e pode designar a classe de saída `View`. JSON e arquivo são outras classes de saída, com contratos diferentes.

## Classes, composição e uma exceção deliberada

As diretrizes originais favorecem código organizado em classes e composição em vez de cadeias de herança. O núcleo usa várias classes estáticas, como `Config`, `Router` e `Galastri`; bancos, validadores e permissões também oferecem objetos instanciáveis.

Há heranças concretas que não devem ser apagadas da explicação: controllers da aplicação estendem `galastri\modules\Controller`, e a exceção do framework estende `\Exception`. A orientação filosófica de preferir composição não significa que o repositório proíba toda herança.

Traits compartilham implementação entre classes: `CoreDatabase` atende os dois drivers; `CoreValidation`, `EmptyValidation` e `ListValidation` atendem validadores. Uma mudança nessas traits possui alcance maior que uma alteração em um único método especializado.

## Separação de responsabilidades

| Responsabilidade | Componente principal |
| --- | --- |
| Preparar o processo | `public/index.php` e bootstrap |
| Armazenar e validar configurações | `Config` e `Definition` |
| Interpretar a árvore da URL | `Router` |
| Coordenar o atendimento | `Galastri` |
| Executar métodos e reunir retornos | `Controller` |
| Transformar resultado em resposta | `View`, `Json`, `File` |
| Registrar e apresentar falhas | `ErrorHandler`, `Exception`, `Message` |

## Implicações para uma contribuição

Preserve as convenções públicas ou explique a migração. Mostre quando uma convenção deriva de intenção registrada e quando deriva apenas da implementação atual. Uma propriedade estática, por exemplo, demonstra compartilhamento de estado na requisição; não demonstra suporte a workers persistentes com várias requisições no mesmo processo.

O objetivo do curso é tornar essa distinção visível: entender a ideia do autor e verificar como ela foi realizada, inclusive onde a implementação ainda exige cuidados.

## Onde conferir no código

- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)
- [`galastri/modules/Database.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Database.php)
- [`galastri/modules/Validation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Validation.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
