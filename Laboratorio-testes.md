> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# teste.php — reproduções pequenas e verificações úteis

## Um arquivo fixo para estudar uma responsabilidade

Use `teste.php` na raiz da cópia local. Carregue constantes e autoload, e inicialize somente as partes necessárias. Isso torna claro se você está testando uma ferramenta, a camada de configuração ou uma requisição completa.

Evite depender de `assert()` como única confirmação, porque sua execução pode ser desativada pela configuração do PHP. Um `if` que lança uma falha é explícito.

## Exemplo completo e repetível

```php
<?php
require __DIR__.'/galastri/bootstrap/constants.php';
require __DIR__.'/vendor/autoload.php';

use galastri\core\config\Config;
use galastri\extensions\Exception;
use galastri\modules\Database;
use galastri\modules\Tools;
use galastri\modules\Validation;

function verificar(bool $condition, string $label): void
{
    if (!$condition) {
        throw new \RuntimeException('FALHOU: '.$label);
    }

    echo 'OK: '.$label.PHP_EOL;
}

Config::run();
Config::set('title', 'Laboratório');
verificar(Config::get('title') === 'Laboratório', 'atribuição de configuração');

$text = Tools::replacePlaceholders('/arquivo%20teste/{{id}}', ['id' => 42]);
verificar($text === '/arquivo%20teste/42', 'placeholders preservam percentuais');

$failed = false;

try {
    Validation::string()->denyEmpty()->validate('   ');
} catch (Exception $e) {
    $failed = true;
}

verificar($failed, 'valor vazio é rejeitado');
Validation::number()->valueRange(1, 100)->validate('50');
echo 'OK: número textual dentro da faixa.'.PHP_EOL;

$db = Database::createSqlite()->setLocation(':memory:')->connect();
$db->query('CREATE TABLE itens (id INTEGER PRIMARY KEY, nome TEXT NOT NULL)')->submit();
$db->query('INSERT INTO itens (nome) VALUES (:nome)')->bind(':nome', 'Exemplo')->submit();
$db->clearBind();
$db->query('SELECT nome FROM itens')->submit();
verificar($db->hasResult(), 'consulta encontra a linha');
verificar($db->getResult()['data'][0]['nome'] === 'Exemplo', 'consulta preserva o conteúdo');
$db->disconnect();

echo 'Laboratório concluído.'.PHP_EOL;
```

Execute `php teste.php`. Cada etapa deve produzir seu `OK` e o arquivo deve chegar à mensagem final. Uma exceção interrompe o teste com informação sobre a etapa divergente.

## Para testar falhas

Capture a classe esperada e confira mensagem ou código estável. Capture `\Throwable` quando o objetivo for distinguir também erros PHP; não use um catch amplo para declarar sucesso independentemente do que aconteceu.

Uma boa reprodução possui um valor que deve falhar e um caso próximo que deve continuar passando. Na validação de vazio, por exemplo, espaços devem ser rejeitados e a string `'0'` deve continuar admitida pela regra específica.

## Para testar requisições

Não chame repetidamente `Galastri::run()` no mesmo processo e suponha que cada chamada começa sem estado. Use o servidor local e requisições independentes, ou processos separados preparados para cada cenário.

No HTTP, confira status, cabeçalhos e corpo. Um `curl` que termina com exit code zero pode ter recebido HTTP 500; isso não confirma o resultado funcional. Em streaming, confira linhas intermediárias e resultado final, além do encerramento da conexão.

## O que este commit não fornece

Não há scripts de teste PHPUnit ou uma pipeline de testes declarada no Composer desta edição. Os laboratórios são um ponto de partida para verificações reproduzíveis, não uma afirmação de cobertura automática já existente no repositório.

Antes de um commit de contribuição, revise `git status --short` e não inclua o laboratório, banco SQLite ou credenciais por acidente.

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`galastri/core/Galastri.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php)
- [`galastri/modules/Database.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Database.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
