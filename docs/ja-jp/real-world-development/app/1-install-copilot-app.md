---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、作成した repository を接続して workspace を確認し、簡単な chat を試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、agent 主導の開発に使う desktop application です。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、repository、branch、CI pipeline をすぐに利用できます。複数の agent をそれぞれ独立した workspace で並行して動かし、作業を指示して、繰り返し作業を自動化する workflow 向けに設計されています。Node.js のインストールと project の準備ができたので、app をインストールして repository を接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールして sign in します。
- GitHub repository から project を app に追加します。
- template で用意された backlog を含め、workspace を確認します。
- app について知るために、簡単な chat を試します。

## シナリオ

チームでは、増え続ける backlog に対応するため AI agent を導入しています。Copilot app を使うと、Issue の選択、agent の実行、変更のレビュー、pull request のマージを一か所から指示できます。このレッスンでは、app をインストールして接続し、project についての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot plan が必要です。Copilot Free、Copilot Student、または有料 plan（Pro、Pro+、Business、Enterprise）を利用してください。Copilot Business または Copilot Enterprise を使う場合、管理者が **GitHub Copilot app** policy を有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まず app をインストールします。Windows、macOS、Linux 向けの version があります。app をインストールして認証し、Tailspin Toys repository を追加しましょう。

1. browser で [GitHub Copilot app の landing page][download-app] を開きます。
2. 使用中の platform 向け app を download し、landing page の案内に従ってインストールします。
3. インストール後、app を開きます。
4. **Sign in to GitHub** を選択し、画面の案内に従って GitHub.com または GHE.com account で sign in します。data residency を使用する GitHub Enterprise Cloud の場合は **GitHub Enterprise Cloud** を選び、prompt が表示されたら `*.ghe.com` の hostname を入力します。

   GitHub Enterprise Server を使用している場合は、まず cloud account で onboarding を完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server の sign-in guide][ghes-sign-in]に従って、browser で app を承認します。

5. onboarding で repository を選択する画面が表示されたら、template から作成した Tailspin Toys repository を選択します。repository の選択を省略して、onboarding 後に project を追加することもできます。
6. prompt が表示されたら theme を選択し、onboarding を完了します。
7. sidebar の **Sessions** に repository が表示されていることを確認します。すでに一覧にある場合、再度追加する必要はありません。
8. repository がまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しい session を作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた owner 名と repository 名を使います。
9. sidebar に repository が表示されることを確認します。この layout では、`tailspin-toys` などの repository folder が **Sessions** の下にまとまり、その下に個々の session が表示されます。

## workspace を確認する

project を接続したら、app の各領域を確認しましょう。sidebar には、主に次の項目があります。

- **New** - Copilot との新しい chat session を開始します。
- **Pull requests** - app の GitHub 統合で pull request を表示します。app を離れずに PR の参照や絞り込み、CI 状態の確認、レビューができます。
- **Issues** - app の GitHub 統合で Issue を表示します。Issue の参照や絞り込み、Issue からの session 開始ができます。
- **Automations** — schedule またはオンデマンドで実行する保存済み agent task です。todo list の管理、定期的な project の保守、または委任したい作業に役立ちます。振り返りでは次の手順として紹介し、別の演習にはしません。
- **Customize** - MCP server、plugin、skill などの component を追加して Copilot app の機能を拡張します。Playwright MCP の設定に使用します。
- **Chats** — branch や専用 workspace が必要ない、質問やアイデア出し向けの簡易的な会話です。最初の chat を始めると表示されます。表示されていない場合は **Sessions** の横の **+** を選択して最初の chat を開始してください。
- **Sessions** — repository folder と agent session をグループ化します。`tailspin-toys` などの repository 名を選択すると repository view が開き、その下の session を選ぶと会話に戻ります。各 session は独立した workspace で実行されるため、変更を競合させずに複数を同時に実行できます。star rating の追加時に最初の session を開始します。

ワークショップを進めながら、workspace を確認します。

> [!TIP]
> 迷ったときは Copilot に質問してください。操作方法がわからない場合や、何かが可能か知りたい場合は、Copilot が案内します。

### 用意された backlog を確認する

backlog のない project はほとんどなく、Tailspin Toys も例外ではありません。template の作成時に用意された backlog を確認しましょう。

1. sidebar の **Sessions** で、Tailspin Toys の repository folder を選択します。その下の個別 session ではなく、repository folder を選んでください。次に repository の上部 navigation から **Issues** を選択します。
2. Issue 番号を決めつけず、次の title で探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue から agent session を開始することもできます。最初の小さな変更の後、filtering issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加した repository の項目のみ表示されます。ほかの repository の作業項目を確認するには、その repository を app に追加してください。

## 簡単な chat を試す

app に慣れるために、app 自体について質問してみましょう。**Quick chat** は、branch や worktree を作らずに質問やアイデア出しができるため、session を必要としない使い捨ての質問に適しています。

1. sidebar で **Chats** の横にある **+** を選択して新しい chat を開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初の chat を開始します。開始後に **Chats** が表示されます。
2. app 自身の session の仕組みについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. conversation view で回答を読みます。各 session が分離された独自の git worktree で動くため、変更を競合させずに複数の agent を並行して動かせることがわかります。会話は続けることも、いつでも新しい chat を始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールして project を接続し、workspace を確認しました。次のことを学びました。

- app をインストールして GitHub に sign in する。
- GitHub repository から project を追加する。
- workspace を確認し、**Issues** で用意された backlog を見つける。
- Quick chat で簡単な質問をする。

次は[最初の agent session を開始し][next-lesson]、ゲームカードに star rating を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の利用を始める][getting-started]
- [GitHub Copilot app で agent session を使う][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
