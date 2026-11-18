---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最も簡単な app の更新でも、通常は複数の file の変更と test の実行が必要です。開発者は、必要な file を探し、変更を行い、test を実行し、debug し、変更し忘れた file を見つけて、さらに更新するという作業を繰り返します。

このような作業で Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者と同様に、最初にプロジェクトの構造を調べ、必要な変更を行い、test などの task を実行して、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で作業し、素早く feedback を得ながら対話できます。cloud agent はチームメンバーのように非同期で作業し、issue と pull request を通じてやりとりします。

この演習では、次のことを学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連 file を特定し、複数の file を協調して変更する方法。
- GitHub Copilot Agent Mode で UI と data layer にわたる新機能を実装する方法。
- Copilot Agent Mode が生成した変更と test を確認してから codebase にマージする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者が category と publisher で絞り込めるようにします。前の演習では publisher helper を作成しました。ここでは、Copilot Agent Mode を使って残りの data layer、UI、test の作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認しましょう。

この Web サイトは、developer theme を持つ board game の crowdfunding platform です。ゲーム一覧と詳細を表示します。サイトは Astro app 1 つで構成され、build 時に各ページを static HTML として render します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite database に query します。独立した backend API や client-side UI framework はありません。再利用可能な data-access helper は `src/lib/` に配置し、操作機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro dev server を起動する `npm run dev` task が用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しい terminal window を開きます。
3. フィルタリング作業用の branch を作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次の command で Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備ができると、次のような URL を示す banner が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開く dialog が表示された場合は、**x** を選択して閉じてください。

5. terminal に表示された Astro server address `http://localhost:4321` を、<kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace の terminal で localhost URL の link を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Web server を実行している Codespace への private tunnel です。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **Home Page**: title、画像、説明を含む board game の一覧を表示します。
- **Game Details Page**: ゲームを選択すると、title、説明、publisher、category などの詳細ページが開きます。

## Copilot で backlog を確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。mode と model の選択は演習ごとに異なるため、該当する手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まず backlog を確認しましょう。template からプロジェクトを作成すると、GitHub issue の backlog が自動的に作成されています。GitHub Copilot に内容を表示してもらいます。

1. Chat view の agents dropdown から **Agent** を選択します。Agent は、file をまたぐ変更を自律的に計画・実装し、terminal command を実行して tool を呼び出します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

2. 利用可能な model の一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定の model を推奨しているわけではありません。ワークショップを作成した際に Claude Sonnet 4.5 を使用したため、手順に含めています。コードの提案に一貫性を持たせ、演習を進めやすくするためです。ただし、LLM は確率的に動作するため、受け取る提案は記載例と異なる場合があります。これは通常の動作であり、想定内です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が異なる MCP command を使う場合がありますが、task は同様に完了できるはずです。

3. 次の prompt を Copilot に送信して、backlog の issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示する command を実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって有益だと考える順に issue を並べたことを確認してください。

## instruction file を確認する

agent にコードを生成させる前に、Copilot に作業のコンテキストを伝える instruction file を確認します。Web サイトの機能追加に関するガイダンスを含む [user interface（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) file を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。内容には次のものがあります。
   - architecture の概要。
   - component design、testability、accessibility の原則。
   - Astro、Tailwind CSS、Drizzle などの file type 向け instruction への link。

> [!TIP]
> instruction file から、別の instruction file やプロジェクト内の file を参照できます。path は instruction file からの相対 path です。これにより、instructions を再利用し、大きな内容を小さく管理しやすい file に分割して、参考になる例を指定できます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも 3 種類の更新が必要です。

- data layer（`src/lib/`）に残りのフィルタリング logic を追加または調整します。
- フィルタリングの動作に関する test を追加または更新します。
- game 一覧ページにフィルタリング UI を追加します。

また、すべての test を実行して成功した後で、変更を codebase にマージします。Copilot Agent Mode にこれらの task を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選択して instruction file を追加します。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat view の agents dropdown で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

4. **Claude Sonnet 4.5** が引き続き model として選択されていることを確認します。
5. backlog の関連 issue に基づいて機能を実装するよう、次の prompt で Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連する file を見つけてから作業を始める様子を確認します。data-layer helper、UI、test を見つけ、file を変更して test を実行します。

   ![Copilot がプロジェクト file を調べている様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、file の変更、test の実行などを行うため、codebase と task の複雑さによって数分かかる場合があります。作業中、editor に **Keep** と **Undo** button が表示されることがあります。Copilot の作業中に選択する必要はありません。作業が終わると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot に求められたら、**Continue** を選択して test を実行します。

   ![Copilot Chat pane で test の実行を確認する dialog](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、test が失敗したりすることがあります。問題ありません。Copilot は code generation と test を繰り返し、task が完了して error を検出しなくなるまで作業します。

   ![Copilot Agent Mode の session が完了した状態](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを起動している browser に戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat window で **Keep** を選択します。

## まとめと次のステップ

Copilot Agent Mode を使って Tailspin Toys Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI と data layer にわたる新機能を実装する方法。
- Copilot Agent Mode でプロジェクトを調べ、関連 file を特定し、複数の file を協調して変更する方法。
- Copilot Agent Mode が生成した変更と test を確認してから codebase にマージする方法。

次は [Playwright MCP server で機能をテストし][next-lesson]、pull request を開きます。

### 追加の演習 — paging を実装する

ゲーム一覧が増えるにつれて、paging が必要になります。この演習で学んだ方法を使って、Copilot に paging の実装を依頼してみましょう。次の点を検討してください。

- 既存の instruction file を使うなど、既存のベスト プラクティスに従います。
- 利用者が page size を選べるようにするか、値を固定するかなど、paging の方法を検討します。
- 希望どおりの実装に必要なガイダンスを prompt に含めます。
- GitHub Copilot に変更を依頼し、context を追加しながら反復する必要がある場合があります。これは Copilot と作業する際の通常の workflow です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
