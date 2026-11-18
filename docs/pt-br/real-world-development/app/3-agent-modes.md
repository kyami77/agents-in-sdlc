---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi desenvolvido para se integrar ao fluxo de trabalho existente de uma organização e ajudar a criar as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará por uma issue para criar um recurso, verificará se o código é válido e se o recurso se comporta como esperado e, por fim, fará a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma sessão durante todo o fluxo de trabalho do recurso. Normalmente, seriam usadas sessões ou PRs diferentes para os vários tipos de arquivo, mas neste workshop faremos um atalho para nos concentrarmos nos conceitos principais.

Para começar, nesta lição você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar o novo recurso no modo **Autopilot**.
- revisar o código.
- validar manualmente o recurso em um canvas de navegador.

À medida que continuar trabalhando no recurso, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá a PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de programar. Você usará o modo Plan para decidir esses detalhes e, depois, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, eles ficam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo parecido:

1. Abra uma issue registrada que descreva o trabalho necessário.
2. Crie um plano do que precisa ser implementado.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie uma pull request (PR).
7. Depois da revisão do código e da aprovação no processo de integração contínua, faça a mesclagem.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria dos processos segue alguma variação desse fluxo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão do código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo do prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você revisa e aprova o plano antes que o agente o execute.
- **Autopilot**: o agente trabalha de forma totalmente autônoma — escreve código, executa testes e itera sem esperar sua orientação.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Iniciar uma sessão a partir da issue

Antes de começar, confirme que a PR das avaliações por estrelas foi mesclada e que sua `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior do repositório, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada no template; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente para executar `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o PATH ou a configuração do gerenciador de versões do aplicativo e feche e reabra o aplicativo completamente. Tente novamente a operação e as verificações que falharam; não reinstale o npm apenas porque o agente não consegue encontrá-lo.

## Planejar o recurso de filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva código. Como você começou pela issue, o Copilot já tem a solicitação do recurso como contexto. Envie:

```plaintext
Implemente esse recurso.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele inclui a filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Converse sobre qualquer comportamento pouco claro, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes unitários, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha-o focado na implementação e nos testes da filtragem; você criará a PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e deixe à mão o URL da issue e quaisquer esclarecimentos acordados para validá-los mais tarde.

## Aprovar explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confirme o modo ativo em vez de presumir que a aprovação o alterou. Verifique se o indicador mostra **Autopilot**.

O Copilot começará a implementar o recurso. Ele seguirá o plano definido de forma iterativa, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revisar e verificar a implementação

Depois que o código for gerado, revise-o antes de mesclar, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de várias categorias e editoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Inspecione os resultados do lint, dos testes unitários, dos testes E2E e da verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e disponibiliza uma pré-visualização e pode reutilizar um servidor local. Confirme se o servidor testado pertence a este worktree, não a uma lição anterior.

## Explorar a nova funcionalidade

O código parece bom, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use o seguinte prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Inicie o aplicativo e abra-o no canvas de navegador.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme se os resultados correspondem a essa editora.
4. Combine filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, use o seguinte prompt para pedir ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Pare o servidor de desenvolvimento e feche o canvas de navegador.
    ```

## Manter esta sessão para as próximas lições

Ainda não crie nem mescle a PR de filtragem. Mantenha esta sessão, este worktree e esta branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem, adicionará melhorias à skill e ao QA e, depois, criará e mesclará a PR combinada na Lição 8. Depois de parar o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para criar e revisar um recurso. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou o novo recurso no modo **Autopilot**.
- revisou o código.
- validou manualmente o recurso em um canvas de navegador.

Em seguida, vamos aprofundar como o código é gerado e garantir que siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
