> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# BrowserCache — validação HTTP e conteúdo autenticado

## Responsabilidade

A trait contém duas rotinas privadas estáticas: política para autenticação e comparação de ETag. Ela é usada pelas classes de saída, mas cada uma escolhe quais rotinas chama.

| Saída | Uso efetivo no commit |
| --- | --- |
| View | Verifica ETag depois da renderização. |
| File renderizado | Verifica ETag depois de carregar o conteúdo. |
| File como download | Aplica a política de autenticação. |
| Json e Json streaming | Aplicam a política de autenticação; não chamam a comparação de ETag. |

## Conteúdo autenticado

Se `Config::get('authTag')` não for vazio, a política envia `Cache-Control: private, no-store` quando cabeçalhos ainda não foram enviados e remove Expires, ETag e Last-Modified naquele momento.

Essa decisão depende da configuração da rota, não de adivinhar se um array contém dados pessoais. Uma resposta personalizada sem `authTag` precisa de sua própria política apropriada.

## Quando a comparação de ETag funciona

A rotina prossegue somente para GET ou HEAD, status 200 (ou ainda não definido) e `browserCache` não vazio. O primeiro componente deve ser um inteiro não negativo; o segundo deve ser string, com padrão `public`.

O código aceita duração `0`, apesar de o texto de uma mensagem de erro falar em “maior que zero”. A condição efetiva é a referência técnica desta edição.

O ETag é um hash MD5 do conteúdo fornecido à rotina. Ele é usado como identificador de versão para cache, não como prova de autenticidade ou assinatura de segurança.

## If-None-Match

O código considera `*`, tags entre aspas, listas e o prefixo fraco `W/`. Se encontrar correspondência válida, define 304 e indica ao chamador que não imprima o corpo.

Uma lista malformada não deve ganhar correspondência por um simples pedaço de texto parecido com o hash. A análise da sintaxe é parte dos testes de regressão relevantes.

## Experiência HTTP

Configure `browserCache => [60, 'public']` numa rota View sem autenticação. Faça uma requisição, copie o ETag recebido e envie outra com `If-None-Match` contendo a tag completa, incluindo aspas.

Espere 304 e nenhum corpo na segunda resposta. Troque o conteúdo e repita: deve voltar a haver resposta com corpo. Use também um POST, que não deve seguir a mesma comparação de cache.

## O que este cache não evita

O controller já executou e o conteúdo de View já foi renderizado antes da comparação. File também já carregou seus bytes. Portanto, 304 economiza transferência de corpo nesse caminho, mas não elimina automaticamente trabalho de banco, arquivos ou template no servidor.

Uma mudança que pretenda armazenar resultados de computação precisa de outro contrato e invalidação; não é somente aumentar o tempo de `browserCache`.

## Onde conferir no código

- [`galastri/core/output/traits/BrowserCache.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/traits/BrowserCache.php)
- [`galastri/core/output/View.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/View.php)
- [`galastri/core/output/Json.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php)
- [`galastri/core/output/File.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/File.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
