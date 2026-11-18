---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、browser でフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したとおり、コードを書くことだけが開発ではありません。data、外部 service を扱い、Copilot が利用できる自動化機能を追加する必要もあります。そこで MCP server を使います。MCP server を使うと、Copilot は CLI に組み込まれた機能だけでなく、さらに多くの tool や service を利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol（MCP）とは何か、Copilot CLI がどう使うかを学びます。
- Playwright MCP server がまだ利用できない場合は追加します。
- agent に browser を操作してフィルタリング機能を試してもらいます。

## シナリオ

unit test と end-to-end test は重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が開発中の Web サイトを利用者と同じように操作できるようにし、変更が期待どおりに動作する確信を高めたいと考えています。

## Model Context Protocol（MCP）とは

[Model Context Protocol（MCP）][mcp-blog-post] は、AI agent が外部の tool や service と通信するための仕組みです。MCP を使うと、AI agent は外部の tool や service とリアルタイムに通信し、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

tool と resource は、AI agent と外部の tool や service の橋渡しをする MCP server を通じて利用できます。MCP server ごとに、AI agent が利用できる tool と resource のセットが定義されています。

代表的な MCP server をいくつか紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub repository、issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使った browser automation を利用できます。

ほかにも多くの MCP server があります。GitHub がホストする [MCP registry][mcp-registry] では、MCP server を見つけたり、ecosystem に貢献したりできます。

> [!CAUTION]
> MCP server は、プロジェクトのほかの dependency と同様に扱ってください。利用する前に source code を確認し、publisher を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot が利用者と同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次の section に進みます。

1. Codespace に戻ります。
2. Copilot CLI で次の command を入力し、MCP server の追加 dialog を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type として **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** dialog に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP dialog を終了します。

## Playwright を使って機能を確認するよう Copilot に依頼する

前の手順では、機能が期待どおりに動作することを手動で確認しました。今度は Copilot から Playwright MCP server を使って同じ確認を行います。

1. 次の prompt で、Playwright MCP server を使って機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、MCP server を使うよう Copilot に明示しなくても、Copilot は自分で判断できます。何を使うべきかわかっている場合は、利用する tool を伝えても問題ありません。結果の一貫性を高め、token を節約できます。

2. 機能が動作することを browser で確認するために、Copilot が実行する手順を確認します。
3. 報告を読み、すべてが期待どおりに動作することを確認します。

Copilot は server を起動し、Playwright で Web サイトを操作してから server を停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP server を使い、実際の browser で機能を確認しました。次のことを行いました。

- Model Context Protocol（MCP）とは何か、Copilot CLI がどう使うかを学びました。
- Playwright MCP server がまだ利用できない場合に追加しました。
- agent に browser を操作させ、フィルタリング機能を試しました。

次は、skill と browser tool を組み合わせる[QA custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
