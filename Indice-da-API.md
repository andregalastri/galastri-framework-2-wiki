> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Índice completo de classes, traits e métodos

## Como consultar

Este índice foi extraído das declarações PHP do commit de referência. Mostra a assinatura tal como declarada, normalizando somente espaços e retirando comentários internos da assinatura. Visibilidade e retorno continuam relevantes: um método privado listado aqui é um ponto de manutenção, não uma chamada disponível à aplicação.

Os métodos fornecidos por traits estão no bloco da trait correspondente. Métodos herdados de classes nativas, como os de `\Exception`, não são duplicados como se tivessem sido implementados no projeto. Os capítulos de classe explicam os efeitos, a ordem e as dependências dessas declarações.

## ErrorHandler

`galastri\core\ErrorHandler` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-ErrorHandler) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/ErrorHandler.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function run(): void` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function setErrorHandler(): void` |
| `private static function setExceptionHandler(): void` |
| `private static function registerShutdownFunction(): void` |
| `private static function getErrorCode(bool\|int $errorLevel, mixed $default = false): mixed` |
| `private static function getMessage(string $message, bool $displayError = false): string` |
| `private static function getLine(int $line): int` |
| `private static function getFile(string $file): ?string` |
| `private static function getTrace(array $trace): array` |
| `private static function printError(int\|string $code, string $message, string $file, int $line, array $trace, bool $displayError = false): void` |
| `private static function printErrorJson(array $data): void` |
| `private static function printErrorView(array $data): void` |
| `private static function printErrorFile(array $data): void` |
| `private static function createLogFile(int\|string $code, string $message, string $file, int $line, array $trace, string $currentDate, string $currentTime): void` |
| `private static function getHttpStatusCode(int\|string $code): int` |

</details>

## Galastri

`galastri\core\Galastri` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Galastri) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Galastri.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function run(): void` |
| `public` | `public static function notFound(): void` |
| `public` | `public static function controllerExists(): bool` |
| `public` | `public static function methodExists(): bool` |
| `public` | `public static function httpMethodExists(): bool` |
| `public` | `public static function getControllerResponse(): array` |
| `public` | `public static function getControllerFileContents(): array` |
| `public` | `public static function getControllerNamespace(): string` |
| `public` | `public static function getControllerMethodName(): string` |
| `public` | `public static function getControllerHttpMethodName(): string` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function setTimezone(): void` |
| `private static function checkForceRedirectTo(): void` |
| `private static function checkOffline(): void` |
| `private static function isValidRoute(): void` |
| `private static function checkAuthentication(): void` |
| `private static function setOutputClass(): void` |
| `private static function setControllerNamespace(): void` |
| `private static function setMethodName(): void` |
| `private static function setHttpMethodName(): void` |
| `private static function executeController(): void` |
| `private static function executeOutput(): void` |

</details>

## Router

`galastri\core\Router` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Router) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function run(): void` |
| `public` | `public static function getUrlParts(): array` |
| `public` | `public static function getControllerName(): string` |
| `public` | `public static function getControllerNamespace(): array` |
| `public` | `public static function getDynamicNodeValues(): array` |
| `public` | `public static function getDynamicNode(string $key): string` |
| `public` | `public static function getMethodName(): string` |
| `public` | `public static function isValidRoute(): bool` |
| `public` | `public static function getParameterValues(): array` |
| `public` | `public static function getParameter(string $key): ?string` |
| `public` | `public static function getRootDir(): string` |
| `public` | `public static function getBaseHref(): string` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function configureRootDir(): void` |
| `private static function setUrlParts(): void` |
| `private static function resolveNodes(): void` |
| `private static function processNodeData(string $className): void` |
| `private static function configureRouteProperties(): void` |
| `private static function configureNodeProperties(): void` |
| `private static function configureEndpointProperties(): void` |
| `private static function resolveEndpoints(): void` |
| `private static function resolveParameters(): void` |
| `private static function resolveNamespace(string $partValue): void` |
| `private static function removePrefixOnce(string $string, string $prefix): string` |
| `private static function setConfigIfExists(string $configName ): bool` |

</details>

## Config

`galastri\core\config\Config` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Config) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Config.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function run(): void` |
| `public` | `public static function set(string $name, mixed $value): void` |
| `public` | `public static function get(...$parameters): mixed` |
| `public` | `public static function list(): array` |
| `public` | `public static function listValues(): array` |
| `public` | `public static function exists(string $name, bool $throwException = false): bool` |
| `public` | `public static function notExists(string $name, bool $throwException = false): bool` |
| `public` | `public static function importConfig(string $path): array` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function create(string $name, array $definitions): void` |

</details>

## Definition

`galastri\core\config\Definition` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Definition) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/config/Definition.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function __construct(string $name, array $definitions)` |
| `public` | `public function getName(): string` |
| `public` | `public function getValidTypes(): array` |
| `public` | `public function getInvalidTypes(): array` |
| `public` | `public function getValidValues(): array` |
| `public` | `public function getInvalidValues(): array` |
| `public` | `public function getValidRegex(): string` |
| `public` | `public function getInvalidRegex(): string` |
| `public` | `public function getContext(): string` |
| `public` | `public function setValue(mixed $value): void` |
| `public` | `public function getValue(): mixed` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function setName(mixed $value): void` |
| `private function setValidTypes(mixed $values): void` |
| `private function setInvalidTypes(mixed $values): void` |
| `private function setValidValues(mixed $values): void` |
| `private function setInvalidValues(mixed $values): void` |
| `private function setValidRegex(mixed $value): void` |
| `private function setInvalidRegex(mixed $value): void` |
| `private function setContext(mixed $value): void` |
| `private function isOfType(array\|string $types, mixed $value): bool` |

</details>

## File

`galastri\core\output\File` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-File) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/File.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function run(): void` |
| `public` | `public static function requiresController(): bool` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function getControllerFileContents(): void` |
| `private static function checkFolderExists(): void` |
| `private static function checkFileExists(): void` |
| `private static function checkExtension(): void` |
| `private static function checkMimeType(): void` |
| `private static function setFileContents(): void` |
| `private static function render(): void` |
| `private static function download(): void` |

</details>

## Json

`galastri\core\output\Json` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Json) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/Json.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function run(): void` |
| `public` | `public static function stream(array $data): void` |
| `public` | `public static function isStreaming(): bool` |
| `public` | `public static function requiresController(): bool` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |

</details>

## View

`galastri\core\output\View` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-View) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/View.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function run(): void` |
| `public` | `public static function requiresController(): bool` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function checkViewFile(): void` |
| `private static function checkTemplateFile(): void` |
| `private static function render(): void` |

</details>

## PhpEngine

`galastri\core\output\engines\PhpEngine` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-PhpEngine) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/engines/PhpEngine.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function __construct()` |
| `public` | `public function get(int\|string ...$keyList): mixed` |
| `public` | `public function print(int\|string ...$keyList) : void` |
| `public` | `public function include(string $path): ?bool` |

## BrowserCache

`galastri\core\output\traits\BrowserCache` — trait. [Capítulo](/andregalastri/galastri-framework-2/wiki/Trait-BrowserCache) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/output/traits/BrowserCache.php).


<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private static function applyAuthenticationCachePolicy(): bool` |
| `private static function checkBrowserCache(string $currentContent): bool` |

</details>

## Exception

`galastri\extensions\Exception` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Exception) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/extensions/Exception.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function __construct()` |
| `public` | `public static function hasPlaceholders(string $message): bool` |
| `public` | `public function getData(int\|null\|string $key = null): mixed` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __overload(array $parameters): void` |
| `private function processArrayWithFlags(array $data, array $printFArgs, ?array $additionalData): void` |
| `private function processSimpleArray(array $data, ?array $additionalData): void` |
| `private function processStringWithFlags(string $message, array $printFArgs, array\|int\|null\|string $code, ?array $additionalData): void` |
| `private function processSimpleString(string $message, array\|int\|null\|string $code, ?array $additionalData): void` |
| `private function setMessage(string $message): void` |
| `private function setCode(int\|string $code): void` |
| `private function setAdditionalData(?array $additionalData)` |
| `private function setLine(int $line): void` |
| `private function setFile(string $file): void` |
| `private function setData(array $data): void` |

</details>

## Message

`galastri\language\Message` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Message) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/language/Message.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function get(string $messageConstName): array` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |

</details>

## Authentication

`galastri\modules\Authentication` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Authentication) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function setAuthTag(string $authTag): void` |
| `public` | `public static function create(?string $authTag = null, bool $regenerateIdSession = true): void` |
| `public` | `public static function validate(string $authTag, bool $ipCheck = false): bool` |
| `public` | `public static function setField(string $fieldName, bool\|int\|null\|string $fieldValue, ?string $authTag = null): void` |
| `public` | `public static function getField(string $field, ?string $authTag = null): bool\|int\|null\|string` |
| `public` | `public static function setCookieExpiration(int $seconds, ?string $authTag = null): void` |
| `public` | `public static function remove(string $authTag): void` |
| `public` | `public static function destroy(): void` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private static function startSession(): void` |

</details>

## Controller

`galastri\modules\Controller` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Controller) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Controller.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `final public function __construct()` |
| `protected` | `abstract protected function main(): array` |
| `protected` | `final protected function setProjectName(string $value): void` |
| `protected` | `final protected function getProjectName(): string` |
| `protected` | `final protected function setTitle(string $value): void` |
| `protected` | `final protected function getTitle(): string` |
| `protected` | `final protected function getOutput(): string` |
| `protected` | `final protected function getTimezone(): string` |
| `protected` | `final protected function setTemplateFile(string $value): void` |
| `protected` | `final protected function getTemplateFile(): string` |
| `protected` | `final protected function setViewFolder(string $value): void` |
| `protected` | `final protected function getViewFolder(): string` |
| `protected` | `final protected function setFileFolder(string $value): void` |
| `protected` | `final protected function getFileFolder(): string` |
| `protected` | `final protected function setFileContents(string $contents, string $fileName, string $mimeType): void` |
| `public` | `final public function getFileContents(): array` |
| `protected` | `final protected function setDownloadableFile(bool $value): void` |
| `protected` | `final protected function isDownloadableFile(): bool` |
| `public` | `final public function getResponse(): array` |
| `protected` | `final protected function getParameterValues(): array` |
| `protected` | `final protected function getParameter(string $key): bool\|null\|string` |
| `protected` | `final protected function getDynamicNodeValues(): array` |
| `protected` | `final protected function getDynamicNode(string $key): bool\|null\|string` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function callControllerMethod(): void` |
| `private function callHttpMethod(): void` |
| `private function mergeReturns(): void` |

</details>

## Database

`galastri\modules\Database` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Database) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Database.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function createMysql(?string $dbConfigName = null): MysqlDatabase` |
| `public` | `public static function createSqlite(?string $dbConfigName = null): SqliteDatabase` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |

</details>

## HttpData

`galastri\modules\HttpData` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-HttpData) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/HttpData.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function getValue(string $key): mixed` |
| `public` | `public static function getAll(): array` |
| `public` | `public static function has(string $key): bool` |
| `public` | `public static function getKeys(): array` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private static function getRequestData(): array` |

</details>

## Permission

`galastri\modules\Permission` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Permission) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Permission.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function __construct()` |
| `public` | `public function checkFor(array\|string ...$permissions): self` |
| `public` | `public function at(array\|string ...$permissions): self` |
| `public` | `public function requireAll(): self` |
| `public` | `public function allowAny(): self` |
| `public` | `public function validate(): void` |
| `public` | `public function setFailMessage(string $message, array $replacements = []): self` |
| `public` | `public function reset(): self` |

## Redirect

`galastri\modules\Redirect` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Redirect) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Redirect.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function to(string $location, array $replacements = []): void` |
| `public` | `public static function statusCode(int $statusCode): string` |
| `public` | `public static function ignoreRootDir(): string` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |

</details>

## ResponseStream

`galastri\modules\ResponseStream` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-ResponseStream) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/ResponseStream.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function send(array $data): void` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |

</details>

## Tools

`galastri\modules\Tools` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Tools) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function createDir(string $path, int $chmod = 0777): string` |
| `public` | `public static function writeFile(string $path, string $content, int $mode = INSERT_CONTENT_AT_END, int $chmod = 0777): void` |
| `public` | `public static function typeOf(mixed $value, bool $getClassForObjects = false): string` |
| `public` | `public static function readableImplode(string $separator, string $lastSeparator, array $array): string` |
| `public` | `public static function hasPlaceholders(string $message): bool` |
| `public` | `public static function replacePlaceholders(string $message, array $replacements = []): string` |
| `public` | `public static function toCamelCase(string $string): string` |
| `public` | `public static function toPascalCase(string $string): string` |
| `public` | `public static function chmod(string $path, int\|string $chmod): void` |
| `public` | `public static function deleteLastKey(array $array, int $quantity = 1): array` |
| `public` | `public static function deleteFirstKey(array $array, int $quantity = 1): array` |
| `public` | `public static function arrayAt(array &$array, int $index, bool $remove = false): mixed` |
| `public` | `public static function arrayFlatten(array $list): array` |
| `public` | `public static function import(string $location, mixed $defaultValue): mixed` |
| `public` | `public static function trimLeft(string $string, ...$replacementList): string` |
| `public` | `public static function trimRight(string $string, ...$replacementList): string` |
| `public` | `public static function trimBoth(string $string, ...$replacementList): string` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function validateChmodCode(int\|string $chmod): int` |

</details>

## Validation

`galastri\modules\Validation` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-Validation) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Validation.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function string(): StringValidation` |
| `public` | `public static function number(): NumericValidation` |
| `public` | `public static function dateTime(): DateTimeValidation` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |

</details>

## VarDump

`galastri\modules\VarDump` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-VarDump) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/VarDump.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public static function print(mixed ...$values): void` |
| `public` | `public static function raw(mixed ...$values): void` |
| `public` | `public static function addBacktraceLevel(): void` |
| `public` | `public static function getBacktraceLevel(bool $reset = KEEP_BACKTRACE_LEVEL): int` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function __construct()` |
| `private static function getValues(mixed ...$values): array` |

</details>

## MysqlDatabase

`galastri\modules\database\MysqlDatabase` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-MysqlDatabase) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/MysqlDatabase.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function __construct(?string $dbConfigName = null)` |
| `public` | `public function setPort(string $value): self` |
| `public` | `public function connect(): self` |

## SqliteDatabase

`galastri\modules\database\SqliteDatabase` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-SqliteDatabase) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/SqliteDatabase.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function __construct(?string $dbConfigName = null)` |
| `public` | `public function connect(): self` |

## CoreDatabase

`galastri\modules\database\traits\CoreDatabase` — trait. [Capítulo](/andregalastri/galastri-framework-2/wiki/Trait-CoreDatabase) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/database/traits/CoreDatabase.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function setHost(string $host): self` |
| `public` | `public function setPort(string $port): self` |
| `public` | `public function setDatabase(string $database): self` |
| `public` | `public function setUsername(string $username): self` |
| `public` | `public function setPassword(string $password): self` |
| `public` | `public function setOptions(array $options): self` |
| `public` | `public function setLocation(string $location): self` |
| `public` | `abstract public function connect(): self` |
| `public` | `public function disconnect(): self` |
| `public` | `public function begin(): self` |
| `public` | `public function cancel(): self` |
| `public` | `public function commit(): self` |
| `public` | `public function query(string $queryString, int\|null\|string $label = null): self` |
| `public` | `public function bind(array\|int\|string $bind, mixed $value = null): self` |
| `public` | `public function clearBind(): self` |
| `public` | `public function onError(array\|string $message, int\|string $code, array $additionalData = []): self` |
| `public` | `public function submit(): self` |
| `public` | `public function getResult(int\|null\|string $label = null): array` |
| `public` | `public function getLastId(int\|null\|string $label = null): int` |
| `public` | `public function hasResult(int\|null\|string $label = null): bool` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function throwError(): void` |
| `private function establishConnection(callable $createPdo): void` |
| `private function setResult(array $result, int\|null\|string $label): void` |
| `private function isConnected(): void` |
| `private function getLastInsertId(): int` |
| `private function processResult(\PDOStatement $pdo): array` |
| `private function resolveDatabaseConfig(?string $dbConfigName = null): array` |
| `private function filterPdoExceptionMessage(\PDOException $e): void` |

</details>

## DateTimeValidation

`galastri\modules\validation\DateTimeValidation` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-DateTimeValidation) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/DateTimeValidation.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function minDateTime(DateTime\|string $minDateTime, ?string $format = null): self` |
| `public` | `public function maxDateTime(DateTime\|string $maxDateTime, ?string $format = null): self` |
| `public` | `public function dateTimeRange(DateTime\|string $minDateTime, DateTime\|string $maxDateTime, ?string $format = null): self` |
| `public` | `public function validate(mixed $value, string $format = 'Y-m-d H:i:s'): void` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function createDateTimeFromString(DateTime\|string $value, ?string $format = null): DateTime` |

</details>

## NumericValidation

`galastri\modules\validation\NumericValidation` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-NumericValidation) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/NumericValidation.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function minValue(int\|float $minValue): self` |
| `public` | `public function maxValue(int\|float $maxValue): self` |
| `public` | `public function valueRange(int\|float $minValue, int\|float $maxValue): self` |
| `public` | `public function denyFloat(): self` |
| `public` | `public function denyZero(): self` |
| `public` | `public function denyNegative(): self` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function isFiniteNumericValue(mixed $value): bool` |

</details>

## StringValidation

`galastri\modules\validation\StringValidation` — class. [Capítulo](/andregalastri/galastri-framework-2/wiki/Classe-StringValidation) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/StringValidation.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function minLength(int $length): self` |
| `public` | `public function maxLength(int $length): self` |
| `public` | `public function lengthRange(int $minLength, int $maxLength): self` |
| `public` | `public function allowChars(string ...$chars): self` |
| `public` | `public function denyChars(string ...$chars): self` |
| `public` | `public function allowWords(string ...$words): self` |
| `public` | `public function denyWords(string ...$words): self` |
| `public` | `public function requiredChars(mixed ...$charGroups): self` |
| `public` | `public function denyUpperCase(): self` |
| `public` | `public function denyLowerCase(): self` |
| `public` | `public function isCpf(): self` |
| `public` | `public function isCnpj(): self` |
| `public` | `public function isEmail(): self` |
| `public` | `public function isPhone(): self` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function validateCharset(string $mode, string ...$datasets): self` |
| `private function validateCase(string $mode): self` |

</details>

## CoreValidation

`galastri\modules\validation\traits\CoreValidation` — trait. [Capítulo](/andregalastri/galastri-framework-2/wiki/Trait-CoreValidation) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/CoreValidation.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function __construct()` |
| `public` | `public function onError(string $message, int\|null\|string $code = null, $additionalData = []): self` |
| `public` | `public function validate(mixed $value): void` |

<details>
<summary>Métodos privados — manutenção interna</summary>

| Assinatura declarada |
| --- |
| `private function throwError(): void` |

</details>

## EmptyValidation

`galastri\modules\validation\traits\EmptyValidation` — trait. [Capítulo](/andregalastri/galastri-framework-2/wiki/Trait-EmptyValidation) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/EmptyValidation.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function denyNull(): self` |
| `public` | `public function denyEmpty(): self` |

## ListValidation

`galastri\modules\validation\traits\ListValidation` — trait. [Capítulo](/andregalastri/galastri-framework-2/wiki/Trait-ListValidation) · [Código](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/validation/traits/ListValidation.php).

| Acesso | Assinatura declarada |
| --- | --- |
| `public` | `public function restrictList(int\|string ...$valueList): self` |

## Composição de métodos por traits

| Classes | Implementação compartilhada |
| --- | --- |
| View, Json e File | BrowserCache. |
| MysqlDatabase e SqliteDatabase | CoreDatabase; MySQL especializa o comportamento de porta. |
| StringValidation e NumericValidation | CoreValidation, EmptyValidation e ListValidation. |
| DateTimeValidation | As mesmas traits, com adaptação da chamada de validate para receber formato. |

## Auxiliares de bootstrap

A função global `vardump(...$values)` é declarada em `galastri/bootstrap/functions.php`. Constantes de caminho, versão, MIME e modos de escrita são apresentadas em [Bootstrap](/andregalastri/galastri-framework-2/wiki/Bootstrap). O alias `\VarDump` é criado pelo bootstrap, não pelo Composer por si só.

## Flags de caracteres de StringValidation

As expansões abaixo pertencem ao código, não são entradas regex livres do usuário. Flags de categorias são usadas conforme o modo de validação; em especial, o modo de palavras não acrescenta automaticamente repetição ou uma classe de caracteres em torno dessas expansões.

| Flag | Expressão declarada no array |
| --- | --- |
| `--numbers` | `'0-9'` |
| `--numbersUtf8` | `'\p{Nl}'` |
| `--letters` | `'a-zA-Z'` |
| `--lettersUtf8` | `'\p{L}'` |
| `--words` | `'a-zA-Z\s'` |
| `--wordsUtf8` | `'\p{L}\s'` |
| `--upperLetters` | `'A-Z'` |
| `--upperWords` | `'A-Z\s'` |
| `--upperLettersUtf8` | `'\p{Lu}'` |
| `--upperWordsUtf8` | `'\p{Lu}\s'` |
| `--lowerLetters` | `'a-z'` |
| `--lowerWords` | `'a-z\s'` |
| `--lowerLettersUtf8` | `'\p{Ll}'` |
| `--lowerWordsUtf8` | `'\p{Ll}\s'` |
| `--specialChars` | `self::SPECIAL_CHARS` |
| `--accentedLetters` | `self::UPPER_ACCENTED_CHARS.self::LOWER_ACCENTED_CHARS` |
| `--accentedWords` | `self::UPPER_ACCENTED_CHARS.self::LOWER_ACCENTED_CHARS.'a-zA-Z\s'` |
| `--upperAccentedLetters` | `self::UPPER_ACCENTED_CHARS` |
| `--upperAccentedWords` | `self::UPPER_ACCENTED_CHARS.'A-Z\s'` |
| `--lowerAccentedLetters` | `self::LOWER_ACCENTED_CHARS` |
| `--lowerAccentedWords` | `self::LOWER_ACCENTED_CHARS.'a-z\s'` |
| `--extendedAccentedLetters` | `self::UPPER_EXTENDED_ACCENTED_CHARS.self::LOWER_EXTENDED_ACCENTED_CHARS` |
| `--extendedAccentedWords` | `self::UPPER_ACCENTED_CHARS.self::LOWER_ACCENTED_CHARS.self::UPPER_EXTENDED_ACCENTED_CHARS.self::LOWER_EXTENDED_ACCENTED_CHARS.'a-zA-Z\s'` |
| `--upperExtendedAccentedLetters` | `self::UPPER_EXTENDED_ACCENTED_CHARS` |
| `--upperExtendedAccentedWords` | `self::UPPER_ACCENTED_CHARS.self::UPPER_EXTENDED_ACCENTED_CHARS.'a-zA-Z\s'` |
| `--lowerExtendedAccentedLetters` | `self::LOWER_EXTENDED_ACCENTED_CHARS` |
| `--lowerExtendedAccentedWords` | `self::LOWER_ACCENTED_CHARS.self::LOWER_EXTENDED_ACCENTED_CHARS.'a-zA-Z\s'` |
| `--spaces` | `'\s'` |

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
