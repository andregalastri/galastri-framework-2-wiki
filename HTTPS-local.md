> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# HTTPS local e cookies de autenticação

## Por que este assunto faz parte do laboratório

`Authentication::create()` configura seu cookie de token com `secure => true`, `httponly => true` e `samesite => 'Strict'`. Por isso, um teste de login em HTTP pode não reproduzir o ambiente esperado pelo módulo. Não altere essa política no núcleo apenas para adaptar o laboratório.

O objetivo é fornecer HTTPS com um certificado local confiável. Uma opção é [mkcert, mantido em seu repositório oficial](https://github.com/FiloSottile/mkcert), que cria uma autoridade certificadora local e certificados de desenvolvimento.

## Caminho de preparação

1. Instale mkcert seguindo o procedimento oficial para seu sistema.
2. Execute `mkcert -install` no ambiente cujo armazenamento de confiança será usado.
3. Gere um certificado para os nomes e endereços pelos quais abrirá o laboratório.
4. Configure o servidor web para usar o certificado e a chave.
5. Confirme no navegador que a conexão é confiável antes de testar login.

A chave da autoridade local permite emitir certificados confiáveis no seu computador. Mantenha-a privada e fora do Git. Compartilhe somente o certificado público da autoridade quando precisar instalar a confiança em outro sistema seu.

## Exemplo no Ubuntu

Com mkcert já instalado:

```bash
mkdir -p ~/.local/share/galastri-dev-tls
mkcert -install
mkcert -cert-file ~/.local/share/galastri-dev-tls/localhost.pem -key-file ~/.local/share/galastri-dev-tls/localhost-key.pem localhost 127.0.0.1 ::1
sudo install -m 0644 ~/.local/share/galastri-dev-tls/localhost.pem /etc/ssl/certs/galastri-dev.pem
sudo install -m 0600 ~/.local/share/galastri-dev-tls/localhost-key.pem /etc/ssl/private/galastri-dev-key.pem
```

No Apache, acrescente uma porta `Listen 127.0.0.1:8443` no arquivo local de portas, ative `ssl` com `sudo a2enmod ssl` e crie um VirtualHost para `127.0.0.1:8443`. Reutilize o `DocumentRoot`, `Directory`, handler FPM e logs do exemplo HTTP; dentro do novo VirtualHost, acrescente:

```apache
SSLEngine on
SSLCertificateFile /etc/ssl/certs/galastri-dev.pem
SSLCertificateKeyFile /etc/ssl/private/galastri-dev-key.pem
```

No Nginx, a configuração correspondente usa `listen 127.0.0.1:8443 ssl;`, `ssl_certificate /etc/ssl/certs/galastri-dev.pem;` e `ssl_certificate_key /etc/ssl/private/galastri-dev-key.pem;`, mantendo as demais localizações do capítulo Nginx.

Rode o teste de configuração do servidor antes de recarregá-lo. Abra `https://localhost:8443/`.

## Particularidade do WSL

O armazenamento de confiança do Ubuntu não é o armazenamento do navegador Windows. Gerar um certificado no WSL não faz o Windows passar a confiar automaticamente na autoridade local.

É preciso instalar **o certificado público `rootCA.pem` da autoridade criada** no armazenamento de confiança do Windows conforme o procedimento oficial do mkcert. `mkcert -CAROOT` informa a pasta da autoridade no Ubuntu. Nunca copie `rootCA-key.pem` para o projeto, para a Wiki ou para outra pessoa.

Outra opção é gerar e confiar o certificado com mkcert no Windows e disponibilizar ao servidor WSL somente o certificado de servidor e sua chave. Escolha uma autoridade local e documente qual lado confia nela.

## Teste da autenticação

Use sempre o mesmo nome de host na criação e na validação do cookie: `localhost` e `127.0.0.1` são hosts diferentes. As portas, por si, não separam cookies. Evite manter laboratórios com a mesma tag de autenticação no mesmo host se não quiser que compartilhem nomes de cookies.

O servidor embutido do PHP não fornece TLS diretamente. Para este teste, use o servidor configurado ou um proxy HTTPS local de confiança.

O êxito é observado no armazenamento de cookies e nas requisições seguintes. `create()` gravar a sessão não comprova que o navegador aceitou o cookie.

## Onde conferir no código

- [`galastri/modules/Authentication.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Authentication.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
