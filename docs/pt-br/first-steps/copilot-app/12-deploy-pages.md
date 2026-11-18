---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar a pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público implantando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas cabe a você revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar uma implantação no GitHub Pages.
- revisar o fluxo de trabalho e a pull request antes da mesclagem.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages disponibiliza um URL público sem adicionar um servidor ou dependência de runtime.

## Peça ao Copilot para preparar a implantação

> [!WARNING]
> Criar uma pull request pode confirmar e enviar código. Revise suas configurações de aprovação e o repositório de destino antes de enviar este prompt. Com **Approve all**, não espere uma solicitação de confirmação separada.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare uma implantação do GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e o implante no GitHub Pages depois que as alterações chegarem à branch padrão. Adicione um README que descreva o aplicativo e explique como executá-lo localmente, caso ainda não exista um. Crie uma pull request com essas alterações e informe o link e quaisquer configurações ou permissões do Pages necessárias. Não mescle, não habilite a mesclagem automática, não altere as configurações do Pages nem implante. Pare e aguarde minha revisão.
```

## Revise o fluxo de trabalho e a pull request

1. Confirme que o README só foi adicionado se o repositório ainda não tinha um.
2. Revise as alterações da pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho, os gatilhos e as permissões.
3. Confirme que o fluxo de trabalho implanta a partir da branch padrão, e não de uma pull request ainda não mesclada.
4. Solicite correções, se necessário, e revise o diff atualizado. Aguarde a aprovação nas verificações obrigatórias da pull request antes de autorizar a mesclagem.

## Autorize a mesclagem e a implantação

> [!WARNING]
> O prompt a seguir autoriza o agente a mesclar a pull request revisada, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e a pull request. As configurações de aprovação podem permitir essas ações sem outra solicitação de confirmação.

Substitua `<PR_NUMBER>` pelo número da pull request revisada e envie este prompt separado:

```plaintext
Revisei a pull request <PR_NUMBER> neste repositório e autorizo a mesclagem e a implantação. Habilite o GitHub Pages usando o GitHub Actions, mescle essa pull request somente depois que as verificações obrigatórias forem aprovadas e acompanhe a implantação até ela ser concluída. Informe o URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou mesclar, diga exatamente o que preciso aprovar ou fazer; não informe que a tarefa foi concluída até o site estar no ar.
```

![Diagrama em três etapas: uma pull request de space-quiz mesclada; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a implantação

1. Acompanhe a execução de implantação do GitHub Actions até que seja concluída com êxito.
2. Abra o URL informado e confirme que o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou mesclar a pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site esteja carregando.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue para a [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
