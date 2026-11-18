---
title: "演習 3 - Playwright MCP server で機能を test する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agents →][next-lesson]

agent mode でフィルタリング機能を作成しました。pull request を開く前に、browser で実際に動作することを確かめましょう。自分で app を操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際の browser を操作して機能を test してもらいます。その後、branch を公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol（MCP）とは何か、MCP server が Copilot に tool を追加する方法を学びます。
- Playwright MCP server を workspace に追加します。
- Copilot に Playwright MCP server を使わせて、browser でフィルタリング機能を手動で test します。
- branch を公開し、フィルタリング作業の pull request を開きます。

## Model Context Protocol（MCP）とは

agent mode は editor の外に接続できると、より多くの機能を利用できます。Copilot がそのために使う仕組みが Model Context Protocol（MCP）です。agent が外部の tool や service と通信するための標準です。

![agent mode の内部構造と、コンテキスト、LLM、MCP server や VS Code extension が提供する tool との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI agent が外部の tool や service と通信する方法を提供します。MCP を使うと、AI agent は外部の tool や service とリアルタイムに通信できます。最新情報（resource）にアクセスし、ユーザーに代わって操作（tool）を実行できます。

これらの tool と resource は、AI agent と外部の tool や service の橋渡しをする MCP server を通じて利用できます。MCP server は、既存の API や NPM package などのローカル tool と AI agent の通信を管理します。MCP server ごとに、AI agent が利用できる tool と resource のセットが異なります。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub repository を管理する API にアクセスできます。repository の作成・更新、issue や pull request の管理などを AI agent が実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使った browser automation を利用できます。Web page の移動、form の入力、button の操作などを AI agent が実行できます。

ほかにも多くの MCP server があります。GitHub がホストする [MCP registry](https://github.com/mcp) では、ecosystem の MCP server を見つけたり、貢献したりできます。

> [!CAUTION]
> セキュリティ面では、MCP server をプロジェクト内のほかの dependency と同様に扱ってください。利用する前に source code を確認し、publisher を検証して、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密 resource や操作へのアクセスを付与する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` file では、この VS Code workspace で利用できる MCP server を構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、すでに `github` server が構成されています。

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

`github` entry はプロジェクト template に含まれています。そのため、前の演習で Copilot が backlog issue を読み取れました。[remote GitHub MCP server][remote-github-mcp-server] を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が最初に tool を使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は Copilot が操作できる browser を提供します。機能を test するのに役立ちます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` entry を追加し、file を次のようにします。

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

2. file を保存します。

remote `github` server とは異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag は browser を画面に表示せずに実行します。画面のない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys project では end-to-end test に Playwright をすでに使っているため、必要な browser も通常はインストール済みです。Copilot が browser がないと報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## server を起動して信頼する

VS Code は MCP server を**必要に応じて起動**します。Copilot が server の tool を初めて使うときに VS Code が起動し、server を信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` entry の上に **Start** action が表示されます。選択して server を起動し、設定を確認します。
2. VS Code から server を信頼するか確認されたら、構成を確認して server を信頼する設定を選びます。

手動での起動は任意です。省略した場合は、次の手順で Copilot が tool を初めて使うときに VS Code が server を起動し、信頼を確認します。

## フィルタリング機能を test する

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot は実際の browser を操作できます。自分で app を操作しなくても、agent が page を開き、移動し、filter を適用して、その結果を読み取って報告します。会話を続けながら機能が期待どおりに動作することを確かめられます。

Playwright MCP server は screenshot ではなく、page の [accessibility tree][playwright-mcp-server] を使って動作します。agent は button、link、list item などの、label 付きの構造化された要素を使って推論します。これは assistive technology の動作と同じなので、簡単な機能確認を accessibility の基本的な確認にも活用できます。

server を接続し app を起動した状態で、次の prompt を使って、Copilot にフィルタリング機能を test してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由で browser を起動し、各手順を実行して確認内容を報告します。issue の受け入れ基準に照らして報告を読み、問題があれば質問したり、pull request を開く前にコード修正を依頼したりします。

> [!NOTE]
> この test を行うには、app を `http://localhost:4321` で実行する必要があります。dev server を停止した場合は、prompt を送る前に再起動します。Copilot が Playwright MCP server を初めて使う際、browser をダウンロードする場合があります。browser がないと報告されたら、`npx playwright install chromium` を実行してから再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## branch を公開して pull request を作成する

機能が動作することを確認できたので、チームのレビュー用に pull request（PR）を開きます。まず `filtering-vscode` branch を公開します。

1. Codespace で **Source Control** panel を開き、Copilot が行った変更を確認します。
2. **+** icon を選択して変更を stage します。
3. **Sparkle** button を使って commit message を生成します。

   ![変更された file を表示する Source Control panel](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して branch を repository に push します。

## pull request を作成する

github.com や GitHub command-line interface（CLI）など、pull request の作成方法はいくつかあります。今回は GitHub Copilot に依頼して、関連する issue を見つけ、その issue と関連付けて PR を作成してもらいます。

1. Copilot Chat panel に移動し、**New Chat** を選択して新しい session を始めます。
2. GitHub tool を使えるよう、agents dropdown で **Agent** が選択されていることを確認します。
3. 次の prompt で PR 作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を取得して操作を実行する際、必要に応じて **Continue** を選択します。GitHub tool を初めて使うときは、GitHub で認証を求められることがあります。画面の案内に従って認証します。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が表示した link を選択して pull request を確認します。まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol（MCP）とは何か、MCP server が Copilot に tool を追加する方法を学びました。
- Playwright MCP server を workspace に追加しました。
- browser でフィルタリング機能を手動で test しました。
- branch を公開し、フィルタリング作業の pull request を開きました。

MCP server を使って機能を*テスト*しました。Copilot を拡張する方法は MCP だけではありません。次は、accessibility review などの作業を効率化する[custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
