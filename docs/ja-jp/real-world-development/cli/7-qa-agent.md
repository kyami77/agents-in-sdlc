---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動チェックを実行し、Playwright MCP で browser のフィルタリング動作を確認しました。今度は、明確な QA プロセスを定義した custom agent に、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- custom agent が instructions、skill、MCP tool と連携する方法を確認します。
- 再利用可能な品質保証 profile を作成して確認します。
- QA agent を選び、フィルタリング issue に照らして報告を確認します。

## シナリオ

Tailspin Toys では、pull request（PR）を開く前に、要件、コード品質、自動チェック、test coverage、browser での動作を一貫して確認したいと考えています。custom agent を使うと QA プロセスを調整し、再利用可能な報告を作成できます。

## custom agent とは

custom agent は、Markdown profile で定義する Copilot の専門的な役割です。profile では、agent の目的、instructions、利用可能な tool を説明します。このワークショップでは、`.github/agents/qa.agent.md` に QA role を定義し、Copilot CLI で選択します。

これまでに使った customization には、それぞれ異なる役割があります。repository instructions はチームの標準を説明します。`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP は browser tool を提供します。QA profile は、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。QA profile は各機能を置き換えるものではなく、別の会話を必要とするものでもありません。

## QA profile を作成する

feature PR を開く前に、再利用可能な QA profile の作成を Copilot に依頼します。この profile では、QA が実行するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive mode であることを確認します。
2. default agent に新しい custom agent の作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## profile を確認する

新しい agent を使う前に、Copilot が意図した QA workflow と境界を profile に反映していることを確認します。これにより、検証だけを依頼したのに profile が不完全だったり、範囲の広い変更を行ったりすることを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、含めると agent を選択するときに名前がわかりやすくなります。
3. profile の instructions を読み、QA が要件を起点とし、repository instructions に従い、`quality-checks` skill を実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更を commit したり pull request を開いたりしないことを確認します。
5. いずれかの役割や制約が欠けている場合は、次に進む前に default agent に修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時に project agent を読み込みます。profile を作成した後、同じフィルタリングの会話を再開して QA を選択すると、issue と計画の内容を引き続き利用できます。

1. 次の prompt を使って agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> agent を作成した直後は、利用可能な agent の一覧に表示されない場合があります。agent は作成されているので、上記の command で有効にできます。

2. 次の prompt で QA agent に機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を開始します。
4. 作業が終わったら、QA agent の報告を確認します。

## まとめと次のステップ

再利用可能な専門的な役割を workflow に加え、その結果を確認しました。このレッスンでは、次のことを行いました。

- custom agent が instructions、skill、MCP tool と連携する方法を確認しました。
- 再利用可能な品質保証 profile を作成して確認しました。
- QA agent を選択し、フィルタリング issue に照らして報告を確認しました。

実装、skill の更新、QA profile、test、検証結果が揃いました。次は、これらを[機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
