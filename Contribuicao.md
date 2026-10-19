> **Aviso:** Este conteúdo foi gerado por IA e está sendo revisado e corrigido por um humano.

# Rotina de contribuição — do achado ao commit revisável

## 1. Definir o problema

Descreva um gatilho observável: uma URL com parâmetro excedente, um JSON inválido, uma transação aberta duas vezes ou um nome de arquivo com acento. Mostre o resultado atual e qual contrato justifica outro comportamento.

Distingua defeito, limitação deliberada e preferência de estilo. Um método diferente do que você imaginou não é necessariamente incorreto; procure seus consumidores e a intenção registrada.

## 2. Fixar a referência

Registre o SHA e a versão PHP. Prepare uma branch local para o trabalho e evite misturar outra alteração pendente. `git diff` e `git status --short` ajudam a mostrar o alcance real.

Para contribuir com o código, trabalhe pelo fluxo que o mantenedor adotar para branches e revisão. Esta edição da documentação foi preparada exclusivamente na Wiki e não estabelece que o repositório já possua uma política automática de pull requests ou integração contínua.

## 3. Reproduzir antes de mudar

Crie `teste.php` para uma responsabilidade isolada ou uma rota pequena para o caso HTTP. O teste deve falhar pelo motivo esperado no código anterior, e não pela ausência do autoload ou de uma extensão.

Inclua um caso próximo que deve continuar passando. Para uma contenção de arquivo, isso pode ser um arquivo interno válido; para vazio, a string zero; para um erro de banco, uma consulta posterior que não reutilize resultado antigo.

## 4. Explicar a alteração exata

Uma orientação revisável informa o arquivo, o método e o trecho que sai e entra. Se a mudança alcançar vários pontos, explique a dependência entre eles. Um novo getter, por exemplo, pode precisar de atualização no consumidor e na documentação.

Mantenha o estilo do projeto e os acordos do mantenedor: exemplos claros com `if/else`, funções legíveis, composição quando apropriada e testes pequenos. Evite transformar uma correção funcional em reformatação extensa de arquivos não relacionados.

## 5. Verificar e registrar limites

Execute o teste da reprodução e os cenários diretamente afetados. Para uma alteração no Router, amplie para nós dinâmicos e File se o algoritmo compartilhado for tocado. Para uma mudança só na redação da Wiki, confira referências e links; não invente uma bateria de testes que espelha texto.

Registre o que não foi executado: uma validação em runtime isolado não demonstra o comportamento de todo proxy real ou de todos os sistemas operacionais.

## 6. Preparar a mensagem de commit

Use um título curto que descreva o resultado, no padrão Conventional Commits. Exemplos de formato:

```text
fix: rejeita parâmetros excedentes na rota
fix: preserva nome UTF-8 no download
docs: explica o fluxo de autenticação por tag
```

O corpo da revisão deve explicar problema, comportamento resultante e validação relevante. O histórico da conversa e tentativas abandonadas só entram quando ajudam o revisor a compreender uma decisão.

## 7. Atualizar a Wiki quando o contrato mudar

Atualize o capítulo conceitual, a referência da classe, os exemplos e a base técnica quando necessário. Alterar apenas uma tabela pode deixar um laboratório ensinando a assinatura antiga.

A Wiki é um repositório Git separado, com URL terminada em `.wiki.git`. Um commit de documentação nela não precisa incluir uma alteração no código da aplicação. Preserve o menu `_Sidebar.md` e o aviso de revisão humana em todas as páginas.

## Critério de uma contribuição concluída

Outra pessoa consegue reproduzir o problema, entender a mudança, verificar o resultado e reconhecer seu limite. Esse é um resultado mais útil que uma alteração grande cujo efeito só o autor consegue explicar.

## Onde conferir no código

- [`composer.json`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/composer.json)
- [`galastri/core/Router.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/core/Router.php)
- [`galastri/modules/Tools.php`](https://github.com/andregalastri/galastri-framework-2/blob/55e91da329a50762a743f838c5899e60fc1e6764/galastri/modules/Tools.php)

---

[Início](/andregalastri/galastri-framework-2/wiki/Home) · [Roteiro de estudo](/andregalastri/galastri-framework-2/wiki/Roteiro-de-estudo) · [Base desta edição](/andregalastri/galastri-framework-2/wiki/Base-desta-edicao)
