> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Validação desta edição — evidências e limites

## Referência e ambiente de verificação

As verificações usam o código do commit `55e91da329a50762a743f838c5899e60fc1e6764`. Os arquivos de exercício foram criados em uma cópia isolada. O repositório principal foi utilizado somente como fonte de leitura.

Os testes PHP foram executados por PHP compilado para WebAssembly em ambiente Node, com acesso a uma pasta local de laboratório. As versões efetivamente reportadas foram **PHP 8.3.33** e **PHP 8.5.10**. Isso exercita a linguagem e os componentes do framework, mas não equivale a uma instalação nativa completa de Ubuntu/Apache/FPM ou Windows/WSL 2.

## Resultado das verificações

| Grupo | Escopo | Resultado |
| --- | --- | --- |
| Núcleo, respostas e banco inicial | 21 cenários em cada uma das duas versões PHP | 42 execuções aprovadas. |
| Entrada, validação, permissões, ferramentas, transações e sessão | 26 cenários em cada versão | 52 execuções aprovadas. |
| Laboratórios completos e definição adicional | 11 cenários em cada versão | 22 execuções aprovadas. |
| Sintaxe dos exemplos PHP | 64 blocos em cada versão, com contexto mínimo para fragmentos | 128 verificações aprovadas. |
| Parser JavaScript de NDJSON | 5 cenários independentes | 5 verificações aprovadas. |

Ao todo, são **58 cenários funcionais PHP executados nas duas versões**, mais as verificações de sintaxe e do parser. Um teste aprovado pode confirmar uma limitação conhecida; não significa que o framework inteiro esteja livre de defeitos.

## O que os cenários exercitam

- Resolução de nós e endpoint, query string separada, parâmetro ausente e excedente, nó dinâmico e método HTTP adicional.
- Composição superficial dos retornos e o fato de o mapa HTTP não impor 405 sozinho.
- Renderização View e escape de HTML na engine.
- Linhas intermediárias e final de NDJSON, erro antes do início e erro depois do envio.
- Arquivo interno, travessia codificada, byte nulo, link simbólico externo, nome UTF-8 de download e tamanho em bytes.
- ETag e resposta 304 sem corpo, no ambiente de execução controlado.
- Configuração rejeitada e configuração adicional com lista de valores.
- Leitura de JSON, precedência sobre query string, nulo presente, inteiros grandes, JSON malformado e escalar, tipos `+json` e limite do PUT URL-encoded.
- Ordem inversa de mensagens de validação, vazio versus zero, listas estritas, literais de caracteres, flags e regras de data e número.
- Consumo do sinal de exibição da validação, composição de placeholders e erros de formatação tratáveis.
- Modos de permissão e reset, escrita de arquivo em três modos, rollback, segundo begin e leitura tardia dos binds.
- Criação de sessão e cookie da tag, validação numa requisição seguinte e remoção da tag.

O cenário de buffer não removível confirmou o ramo de erro de classe ausente descrito em [Limites conhecidos](/andregalastri/galastri-framework-2/wiki/Limites-conhecidos). Ele foi registrado como comportamento observado; nenhum código foi corrigido durante a edição da Wiki.

## Parser do navegador

O exemplo JavaScript foi exercitado com UTF-8 dividido byte a byte, várias linhas num único chunk, última linha sem quebra final, erro JSON anterior ao streaming e um formato de resposta inesperado.

Isso verifica o algoritmo de decodificação e separação de linhas. Não mede o instante em que um proxy real libera dados nem o tempo de atualização de uma interface.

## Revisão documental e publicação

As verificações de estrutura conferem o aviso no início de cada página, o fechamento de blocos de código, destinos internos e arquivos de código vinculados. O índice de API foi extraído das declarações do código e as tabelas de configuração foram conferidas a partir das definições carregadas.

A publicação usa commits da Wiki. A revisão visual inclui o aviso, o menu lateral customizado, tabelas, exemplos e a separação da área reservada a usuários.

## Verificação adicional da apresentação gráfica de erro

O runtime utilizado possui GD sem FreeType. Ao executar o ramo de erro File com `E_ALL`, `imagettftext()` emitiu um aviso de ausência desse suporte; no PHP 8.5 também foram observados os avisos de depreciação de `imagedestroy()`. A resposta PNG final não foi validada nesse runtime.

Essa verificação adicional registra uma limitação do ambiente e um ponto de compatibilidade do código; ela não está incluída entre os 58 cenários funcionais aprovados. Os cenários de arquivo proibido conferem o status e que o conteúdo externo não tenha sido entregue. Uma resposta gráfica íntegra requer outra verificação com GD/FreeType e a política real de avisos do PHP.

## O que depende do ambiente do colaborador

Os comandos de instalação de sistema, o provisionamento de MySQL/MariaDB, os testes de integração nativa Apache/Nginx/PHP-FPM, a confiança do certificado em cada sistema e o comportamento de aceitação de cookies de cada navegador precisam ser conferidos no ambiente montado pelo colaborador.

Os exemplos de conexão MySQL e a proposta de um método novo em “Como estender” não são apresentados como funcionalidades executadas pelo teste isolado. O primeiro exige um serviço externo configurado; a segunda é um exercício de implementação futura.

Reproduza os casos relevantes ao alterar uma responsabilidade. Não use esta evidência como garantia para uma versão ou um commit que ainda não foram analisados.

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
