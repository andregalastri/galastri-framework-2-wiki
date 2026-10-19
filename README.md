> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Galastri Framework — documentação para mantenedores

Esta Wiki ensina a **entender, investigar e evoluir o Galastri Framework**. O ponto de partida é o código do projeto: acompanhamos como uma requisição vira uma rota, uma configuração, uma chamada de controller e, por fim, uma resposta.

O texto foi reescrito do zero para esta edição. A documentação original fornecida pelo autor foi usada para recuperar a proposta e os objetivos; os contratos técnicos são fundamentados no commit identificado em [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao).

## Para quem é este material

Para quem pretende corrigir um defeito, revisar uma contribuição, implementar um módulo ou entender por que determinada alteração afeta outras partes do framework. Também serve a quem conhece PHP, mas nunca trabalhou no núcleo de um framework.

É útil saber ler classes, arrays, namespaces e exceções. Os capítulos explicam os conceitos específicos do projeto antes de usá-los. Não é necessário começar conhecendo todos os módulos.

## Escolha seu caminho

| Objetivo | Comece por |
| --- | --- |
| Montar o ambiente e abrir o projeto | [Ambiente local](/andregalastri/galastri-framework-2/wiki/Ambiente-local) |
| Estudar o framework em sequência | [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) |
| Entender sua proposta | [Visão geral e filosofia](/andregalastri/galastri-framework-2/wiki/Vis%C3%A3o-Geral) |
| Localizar uma responsabilidade | [Estrutura de pastas](/andregalastri/galastri-framework-2/wiki/Estrutura-de-pastas) |
| Conferir a versão técnica documentada | [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao) |

## O curso em três percursos

**Aprender:** siga o roteiro em ordem, montando o ambiente e acompanhando as experiências. Cada capítulo conecta uma entrada concreta aos componentes que a processam.

**Consultar:** use o menu lateral para ir direto à classe, trait ou configuração. O [índice da API](/andregalastri/galastri-framework-2/wiki/Indice-da-API) reúne as 291 declarações de métodos das 31 classes e traits do commit de referência, incluindo a visibilidade dos pontos internos.

**Contribuir:** use os [laboratórios](/andregalastri/galastri-framework-2/wiki/Laboratorio-testes), o [diagnóstico](/andregalastri/galastri-framework-2/wiki/Diagnostico) e o [roteiro de contribuição](/andregalastri/galastri-framework-2/wiki/Contribuicao) para preparar uma mudança pequena, explicável e verificável. Consulte também a [evidência de validação](/andregalastri/galastri-framework-2/wiki/Validacao-desta-edicao).

## Como os exemplos devem ser usados

Os exemplos de aplicação são laboratórios para observar o núcleo. Quando um capítulo manda criar um controller, uma rota ou `teste.php`, isso significa **na sua cópia local de estudo**. Não significa que esses arquivos já façam parte do repositório ou que esta documentação tenha modificado o código do framework.

Cada experiência deve permitir responder quatro perguntas: qual entrada provocou o comportamento, por quais componentes passou, qual resultado apareceu e o que seria afetado se o contrato mudasse.

## Área de usuários

Reservada para uma documentação futura. Nenhum tutorial de usuário está sendo desenvolvido nesta edição.

## Compromisso desta edição

O comportamento implementado é descrito mesmo quando possui limitações. Uma sugestão de melhoria aparece como sugestão, sem ser apresentada como funcionalidade pronta. O aviso de revisão humana permanece no início de todas as páginas.

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
