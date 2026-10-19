> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Windows — preparar Ubuntu no WSL 2

## O que fica em cada lado

PHP, Composer, Git e os arquivos do projeto ficam no Ubuntu. O navegador e o editor podem continuar no Windows. Os comandos marcados como PowerShell são executados no Windows; os comandos Bash são executados dentro do Ubuntu.

Esta separação evita, por exemplo, instalar uma extensão no PHP do Windows e tentar usá-la no PHP do Linux. São instalações diferentes.

## 1. Instalar a distribuição

Abra **PowerShell como administrador**:

```powershell
wsl --list --online
wsl --install -d Ubuntu-24.04
```

O primeiro comando confirma o nome oferecido no seu computador. Se a distribuição já estiver instalada, não a reinstale; abra a existente. Reinicie quando o instalador solicitar e crie o usuário Linux na primeira abertura.

Confira a versão:

```powershell
wsl --list --verbose
```

A coluna `VERSION` deve indicar `2` para a distribuição usada. Se ela estiver em WSL 1, a conversão é:

```powershell
wsl --set-version Ubuntu-24.04 2
```

Use o nome exibido pela listagem. Uma mensagem sobre virtualização exige conferir os requisitos do WSL e a virtualização do computador; não é um erro do Galastri.

## 2. Abrir o terminal certo

No Windows Terminal, selecione Ubuntu, ou execute no PowerShell:

```powershell
wsl --distribution Ubuntu-24.04
```

Agora confira **dentro do Ubuntu**:

```bash
cat /etc/os-release
pwd
```

O caminho de trabalho recomendado é `~/projetos`. Não clone o laboratório em `/mnt/c/...` se as ferramentas que vão manipulá-lo são as do Linux. A Microsoft recomenda manter os arquivos no sistema de arquivos das ferramentas utilizadas.

## 3. Instalar as ferramentas e obter o projeto

Continue em [Ubuntu — instalação e primeira execução](/andregalastri/galastri-framework-2/wiki/Ambiente-Ubuntu). Esse capítulo é comum a Linux nativo e WSL 2.

## 4. Usar o editor do Windows

Com VS Code instalado no Windows e sua extensão oficial WSL, abra a pasta a partir do Ubuntu:

```bash
cd ~/projetos/galastri-framework-2
code .
```

Confirme no editor que a janela está conectada ao WSL. O terminal integrado deve encontrar o mesmo `php` e `composer` usados no Ubuntu. A extensão WSL instala o componente remoto necessário; não é preciso copiar manualmente o projeto para o Windows.

Outros editores podem ser usados, desde que preservem UTF-8 e a caixa dos nomes dos arquivos. Para abrir a pasta no Explorer a partir do Ubuntu:

```bash
explorer.exe .
```

## 5. Acessar a aplicação

Deixe o servidor PHP em execução no Ubuntu e abra `http://localhost:8080/` no navegador do Windows. O encaminhamento por localhost é o caminho usual do WSL. Se falhar, compare com `curl` executado dentro do Ubuntu para separar problema de aplicação de problema de acesso Windows/WSL.

Não abra portas no roteador para este laboratório. O curso usa acesso local. Para testar cookies Secure e login, passe depois para [HTTPS local](/andregalastri/galastri-framework-2/wiki/HTTPS-local).

## Problemas frequentes

| Sintoma | Primeira conferência |
| --- | --- |
| `php` não encontrado | O pacote foi instalado dentro do Ubuntu ou somente no Windows? |
| Classe encontrada no Windows e ausente no Linux | Nome e caixa do arquivo correspondem ao namespace? |
| PHP funciona, mas a janela do terminal fecha e o site para | O servidor embutido depende daquele processo ativo. |
| Navegador não abre, mas `curl` no Ubuntu funciona | Confira o encaminhamento localhost e a porta escolhida. |
| Login funciona no servidor HTTPS e falha no laboratório HTTP | `Authentication` cria cookie com `secure => true`. |

## Fontes oficiais

- [Instalar WSL](https://learn.microsoft.com/en-us/windows/wsl/install).
- [Preparar ambiente de desenvolvimento](https://learn.microsoft.com/en-us/windows/wsl/setup/environment).
- [Trabalhar entre sistemas de arquivos](https://learn.microsoft.com/en-us/windows/wsl/interop).

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
