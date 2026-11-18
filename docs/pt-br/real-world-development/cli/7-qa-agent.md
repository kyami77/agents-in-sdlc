---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem no navegador. Agora, combinará esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as conclusões com base na issue de filtragem.

## Cenário

Antes de abrir uma pull request (PR), a Tailspin Toys quer uma revisão consistente dos requisitos, da qualidade do código, das verificações automatizadas, da cobertura de testes e do comportamento no navegador. Um agente personalizado pode coordenar esse processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá uma função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até agora têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA informa ao Copilot como usar esses recursos para avaliar os requisitos e apresentar as conclusões. Ele não substitui esses recursos nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir a PR do recurso, peça ao Copilot para criar um perfil de QA reutilizável. O perfil definirá as verificações de QA e os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão para criar o novo agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar os recursos de acordo com suas issues e os requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o MCP do Playwright para verificar o comportamento e adicionar testes quando faltar cobertura.

   O agente deve marcar cada requisito como aprovado, reprovado ou bloqueado e apresentar evidências. Ele precisa pedir autorização antes de alterar o código de implementação e não pode confirmar alterações nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecionar o perfil

Antes de usar o novo agente, revise o perfil para confirmar se o Copilot registrou o fluxo de QA e os limites desejados. Isso evita um agente incompleto ou abrangente demais, que altere o recurso quando você só quer verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia os metadados. O campo `description` é obrigatório; `name` é opcional, mas dá ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme se o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme se o QA apresenta evidências, pede autorização antes de alterar o código de implementação e não confirma alterações nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou algum desses limites, peça ao agente padrão que o revise antes de continuar.

## Executar o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Retome a mesma conversa de filtragem depois de criar o perfil e selecione o agente de QA para que ele possa usar a issue e as decisões de planejamento já presentes no contexto.

1. Ative o agente com o seguinte prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, ele talvez ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use o prompt a seguir para pedir ao agente de QA que revise o recurso:

   ```plaintext
   Revise o recurso de filtragem com base na issue e nas decisões do nosso plano. Ele está pronto para uma PR?
   ```

3. O agente de QA começará o trabalho.
4. Quando o agente terminar, leia o relatório.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável e revisou o trabalho do agente. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as conclusões com base na issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você [reunirá esses itens em uma PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
