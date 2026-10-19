> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Núcleo — responsabilidades e fronteiras

## Como as peças se relacionam

O núcleo conhece as convenções da aplicação, mas não o domínio de negócio. Ele sabe resolver classes, aplicar configurações e escolher a saída; não sabe o que significa “cliente ativo”, “saldo disponível” ou “usuário autorizado a editar este registro”.

| Componente | Entrada | Saída ou efeito |
| --- | --- | --- |
| `Config` | Arquivos de definição e valores | Objetos Definition e valores validados. |
| `Definition` | Contrato de uma configuração | Aceitação ou exceção ao atribuir valor. |
| `Router` | URI e árvore de rotas | Namespace, método, parâmetros e configuração acumulada. |
| `Galastri` | Resultado do Router e configuração | Verificações, criação do controller e execução da saída. |
| `ErrorHandler` | Erro PHP ou exceção não capturada | Status, apresentação da falha e log, conforme o estado disponível. |
| Classes de saída | Resposta do controller ou arquivo físico | Corpo e cabeçalhos HTTP. |

## Dependência de ordem

`Config` deve existir antes de o Router atribuir configurações. O Router deve resolver nomes antes de `Galastri` verificar o controller. O construtor de `Controller` precisa dos métodos que `Galastri` já selecionou. A saída consome o resultado depois da construção do objeto.

Essa ordem explica por que chamar isoladamente uma classe estática do núcleo pode falhar em um teste que não preparou o contexto. Um teste de `Router` precisa de configuração e variáveis da requisição; um teste de `Tools::replacePlaceholders()` não precisa atender a aplicação inteira.

## Estado durante a requisição

As propriedades estáticas guardam dados como parâmetros, nome de método, saída e estado do streaming. Não há nesta edição um reset geral destinado a servir múltiplas requisições sucessivas no mesmo processo PHP persistente.

Para testes de integração de URLs diferentes, inicie uma requisição ou processo separado por cenário. Chamar `Galastri::run()` repetidamente dentro do mesmo `teste.php` não reproduz automaticamente o ciclo normal de um servidor PHP.

## Como avaliar uma mudança no núcleo

Ao mudar o nome resolvido de um método, procure seus consumidores em `Galastri`, `Controller` e `View`. Ao mudar um valor de configuração, procure sua definição, onde o Router o aplica e onde a saída o consulta. Ao mudar uma exceção, considere a formatação de mensagens e o próprio ErrorHandler.

O capítulo de contribuição organiza esse raciocínio como uma rotina de manutenção.

## Onde conferir no código

- [`galastri/core/config/Config.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Config.php)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/modules/Controller.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
