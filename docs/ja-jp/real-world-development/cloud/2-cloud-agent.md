---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instructions][previous-lesson] |
|:--|

多くの組織が技術的負債に悩まされています。未解決のセキュリティ問題、更新が必要なレガシー code、実装する時間がなく backlog に残った機能 request などが例です。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできる draft PR を作成します。時間のかかるタスクを委任することで開発を速め、開発者はより重要な作業に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確にすることの重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には対応が必要な技術的負債があります。最初の Web サイトを作成した委託先は、ドキュメントを十分に作成していませんでした。まず、アプリケーションの export 関数に TSDoc doc comment を追加します。

さらに、design team はゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを表示し、利用者がほかのゲームも引き続き見つけられるようにします。まだ完成度の高い実装は不要で、UX の受け入れテストに使えるものを求めています。この作業は現在 blocker ですが、より優先度の高い issue がほかにあります。

このような作業は、優先順位が下がることがあります。Copilot cloud agent に割り当てれば、agent が非同期に作業している間、開発者は別の作業を進められます。その後、戻って Copilot の作業をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様に、バックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進捗を記録する draft pull request を作成し、環境をセットアップして、タスクに取りかかります。作業中または完了後に Copilot のセッションを確認できます。提案された解決方法のレビュー準備ができると、pull request で通知されます。

## 範囲を適切に定めた instructions の重要性

GitHub Copilot に魔法はありません。考える必要がなくなるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲に注意してください][cloud-agent-best-practices]。AI pair programmer として扱い、段階を踏んで学び、試し、状況に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## この repository の custom instructions

前の演習では、custom instructions と、Copilot を導く方法を確認しました。custom-instructions の演習をすでに行った場合は、`.github/copilot-instructions.md` を使った効果を確認しています。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、repository に含まれる instruction file を読み取り専用で確認して、後でその影響を探せるようにします。

repository の GitHub web UI、または実行中の Codespace で次の file を開きます。

- `.github/copilot-instructions.md` — **Code standards** section、特に TypeScript の慣例と TSDoc doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` file を対象にすることを確認します。

次の section では、`Code lacks documentation` issue を cloud agent に割り当てます。生成された pull request で TSDoc doc comment、TypeScript の慣例、comment header を確認してください。これらの内容は instruction file によるものです。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成では、誰が担当する場合でも、通常は特定の環境と初期設定が必要です。Copilot にタスクを割り当てる場合も同様です。Copilot は software engineer と同じように作業します。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用の setup workflow][setup-workflow]を作成すると、作業開始前に必要な開発 tool や dependency を利用できるように環境をカスタマイズできます。この workflow は、lab の進行と学習のため、あらかじめ設定されています。単一の Astro app 用に、Node.js、プロジェクト dependency、end-to-end test 用の browser binary、migration と seed 済みの SQLite database を Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# repository の "Actions" tab から setup step を test できます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各 step* に必要な最小限の permission を設定します。Copilot の操作には専用 token が付与されます。
    permissions:
      # dependency のインストールなど、setup step の一部として repository を clone する場合は `contents: read` permission が必要です。setup step で clone しない場合は、完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app のセットアップ - Node.js（app 全体で Astro + Drizzle/libSQL を使用）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # build と test でデータを利用できるよう、ローカル SQLite database を migrate して seed します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow file と似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` という job が 1 つあります。この job は、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger を使うと、repository の **Actions** tab から workflow を手動で実行できます。Copilot が実行するまで待たずに workflow の成功を確認できるため便利です。

## ドキュメントを追加する

ドキュメントが重要なのは周知のとおりですが、多くのプロジェクトでは情報が古かったり、ドキュメント自体がなかったりします。このような技術的負債が放置されると、生産性が低下し、codebase の保守や新しい開発者の参加が難しくなります。Copilot はドキュメント作成が得意なので、cloud agent に依頼するのに適した issue です。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で結果を確認します。

1. 新しい browser tab で github.com の repository に移動します。
2. **Issues** tab を選択します。
3. **New issue** を選択して、新しい issue dialog を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当て dialog を開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** tab を選択します。
11. 新しく作成された pull request（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。一覧に PR が表示されない場合は、少し待ってから browser を更新してください。
12. 数分後、Copilot が todo list を作成します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかることがあります。Copilot は環境を準備し（前に説明した workflow を実行）、プロジェクトを分析して、問題への最適な対応方法を決めています。

13. todo list を確認し、Copilot が実行するタスクを確認します。
14. pull request の timeline を下にスクロールすると、Copilot が issue の作業を開始したことを示す更新があります。
15. **View session** button を選択します。

  ![Copilot session view](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示の更新を確認するには、browser window を更新する必要がある場合があります。

16. 実行中の session を確認します。Copilot がコードを調査して状態を把握し、計画を考えて選択し、コードを作成する様子を確認できます。

これには数分かかります。Copilot cloud agent の主な利点は、非同期に作業できることです。別の作業を進める間に、Copilot cloud agent にもう 1 つのタスクを割り当て、アプリケーションの機能を追加するコードも書いてみましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分けられることです。design team 向けの関連ゲーム section の追加に時間がかからない場合でも、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com の repository に戻ります。
2. **Issues** tab を選択します。
3. **New issue** を選択して、新しい issue dialog を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当て dialog を開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、issue の最初の comment に 👀 が表示されます。Copilot が作業を開始したことを示します。

![Copilot が issue に対応中であることを示す目の emoji](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の request に取り組んでいます。Copilot cloud agent は software engineer のように作業するため、常に監視する必要はなく、完了後にレビューできます。続けて custom agent の作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期で作業を進められます。技術的負債への対応、新しい機能の作成、別の framework からの code migration などに利用できます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確にすることの重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、custom agent の作成と使用を確認します。[Copilot cloud agent は MCP server も使用できます][cloud-agent-mcp]。また、前の module で確認した custom instructions も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agents →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
