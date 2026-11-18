---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir uma pull request, confirme se ele realmente funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe que o Copilot controle um navegador real para testar o recurso. Depois, publique sua branch e abra a PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem no navegador.
- publicar sua branch e abrir uma pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é a maneira como o Copilot faz isso: um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e sua interação com o contexto, o LLM e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais — por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub. Ele permite que o agente de IA crie repositórios, atualize repositórios existentes e gerencie issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegadores com o Playwright. Ele permite que o agente navegue em páginas Web, preencha formulários e selecione botões.

Há muitos outros servidores MCP que oferecem acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cautela ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do template do projeto. Por isso, o Copilot conseguiu ler as issues do backlog no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]; não é necessário instalar nada localmente. O VS Code conecta-se ao servidor por HTTP e, na primeira vez que o Copilot usa uma de suas ferramentas, você se autentica no GitHub.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, ideal para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` junto à entrada `github`, de modo que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` faz com que o Playwright execute o navegador sem uma janela visível. Isso é necessário em um Codespace, onde não há uma área de trabalho para exibir a janela.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador disponível, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha confiar nele para permitir a inicialização.

Iniciá-lo manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá que você confie nele na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de você testar o aplicativo manualmente, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É a maneira mais rápida de confirmar se o recurso funciona como esperado sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente analisa elementos estruturados e identificados — botões, links e itens de lista — da mesma forma que a tecnologia assistiva. Portanto, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar o recurso de filtragem que você acabou de criar:

```text
Usando o servidor MCP do Playwright, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e observe quantos jogos aparecem.
2. Aplique um filtro de categoria e confirme que a lista é atualizada para mostrar somente jogos dessa categoria.
3. Limpe o filtro, aplique um filtro de editora e confirme que a lista é atualizada para exibir os jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme se os resultados correspondem a ambos.

Relate o que observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas complementares ou peça que ele corrija o código antes de abrir uma pull request.

> [!NOTE]
> Para este teste, o aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar uma pull request

Agora que confirmou que o recurso funciona, você pode abrir uma pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. No Codespace, abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar a pull request

Há várias maneiras de criar uma pull request, incluindo o github.com e a interface de linha de comando (CLI) do GitHub. Como você já está usando o GitHub Copilot, vamos pedir a ele que crie a PR. Ele pode localizar a issue relevante e associá-la à PR.

1. Abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme se **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar uma PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie uma pull request para a branch atual e associe-a à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se no GitHub. Siga as instruções para autorizá-lo.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria a PR.
6. Selecione o link criado pelo Copilot para revisar a pull request, mas **não a mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou esse servidor para testar manualmente o recurso de filtragem em um navegador antes de publicá-lo.
- publicou a branch e abriu uma pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
