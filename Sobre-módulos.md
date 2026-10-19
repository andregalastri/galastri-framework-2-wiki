> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Módulos — ferramentas disponíveis para a aplicação

## Famílias diferentes de ferramentas

O diretório `galastri/modules/` reúne componentes usados pela aplicação e pelo próprio núcleo. Alguns são fachadas estáticas; outros são objetos com estado. Compartilhar a pasta não significa que todos tenham o mesmo ciclo de vida.

| Módulo | Forma de uso | Responsabilidade |
| --- | --- | --- |
| `Controller` | Herança pela aplicação | Execução do endpoint e composição de retornos. |
| `HttpData` | Métodos estáticos | Leitura e composição de dados da requisição. |
| `ResponseStream` | Método estático | Envio intermediário de arrays para Json. |
| `Validation` | Fábrica estática | Criação de validadores de texto, número e data/hora. |
| Validadores | Objetos fluentes | Armazenar regras e validá-las na ordem implementada. |
| `Database` | Fábrica estática | Criar objetos de drivers PDO. |
| Drivers de banco | Objetos fluentes | Conexão, fila SQL, resultados e transações. |
| `Authentication` | Estado estático e sessão | Tokens por tag e persistência da autenticação. |
| `Permission` | Objeto fluente | Comparar permissões disponíveis e esperadas. |
| `Redirect` | Estado estático | Preparar status, destino e encerramento por redirecionamento. |
| `Tools` | Métodos estáticos | Operações comuns de texto, arrays e arquivos. |
| `VarDump` | Métodos estáticos | Apresentação HTML de dados para depuração. |

## Como escolher o ponto de entrada

Para ler a entrada, comece por HttpData e então valide o que a operação exige. Para consultar dados, crie o driver e abra a conexão explicitamente. Para proteger uma rota, entenda a exigência de `authTag` e depois a diferença entre autenticação e permissão.

Não chame uma rotina do núcleo cedo demais só porque ela é pública e estática. Um módulo pode depender de Config já carregado, de constantes ou de uma sessão. A referência de cada um descreve essas dependências.

## Estado em objetos fluentes

Métodos que retornam `self` normalmente modificam o mesmo objeto e permitem encadeamento. Eles não criam uma nova cópia de configuração a cada chamada. Isso explica os binds de Database, a cadeia persistente dos validadores e a necessidade de `reset()` em Permission.

Ao investigar uma falha, anote o estado montado **antes** da chamada que executa a operação: `submit()` no banco, `validate()` nos validadores e permissões, `create()` na autenticação.

## Onde conferir no código

- [`galastri/modules/Database.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Database.php)
- [`galastri/modules/Validation.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Validation.php)
- [`galastri/modules/Permission.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Permission.php)
- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
