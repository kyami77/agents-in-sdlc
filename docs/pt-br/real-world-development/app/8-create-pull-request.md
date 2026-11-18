---
title: "Lição 8 - Criar e mesclar a PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisá-los em conjunto e abrir uma pull request. Você mesmo mesclou o pull request (PR) das avaliações por estrelas; desta vez, usará o **Agent Merge** para gerenciar o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluiremos todo esse trabalho nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar toda a PR do recurso e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar a mesclagem da PR.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização de mesclagem sob o controle da pessoa desenvolvedora.

## Introdução ao Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir uma pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê a PR e resolve o que estiver bloqueando a mesclagem — corrige verificações de integração contínua (CI) com falha, responde aos comentários da revisão e faz rebase quando necessário — e mescla assim que o GitHub permite. Ele trabalha em segundo plano, continua após a reinicialização do aplicativo e se desativa quando a pull request é mesclada.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade, mas sua capacidade de editar código e mesclar ainda exige sua autorização explícita. Revise as ações permitidas e o trabalho antes de conceder permissão para mesclar.

## Usar o Agent Merge para gerenciar a PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo da PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge começará e poderá:

- Criar a pull request com título e descrição.
- Se a sessão tiver começado a partir de uma issue, incluir uma referência à issue relacionada no corpo da descrição.
- Fazer rebase ou resolver possíveis conflitos de mesclagem com a branch de destino.
- Acompanhar o processo de CI para confirmar que todas as verificações foram aprovadas.
- Acompanhar a PR para identificar comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer alterações para resolvê-los.
- Fazer a mesclagem automática da PR quando tudo tiver sido aprovado, se você autorizar essa ação.

Vamos permitir que o Agent Merge também mescle a PR quando tudo for aprovado.

5. Revise o diff da PR e as evidências de verificação antes de conceder permissão de mesclagem.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente para mesclar:

    ```plaintext
    Mescle a PR quando todas as verificações e aprovações obrigatórias forem concluídas com sucesso. Não ignore as proteções do repositório.
    ```

9. Acompanhe o progresso e resolva quaisquer bloqueios informados. Confirme se o status da PR é **Merged** antes de continuar; habilitar a permissão de mesclagem ou ter um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou toda a PR do recurso e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou a mesclagem da PR.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
