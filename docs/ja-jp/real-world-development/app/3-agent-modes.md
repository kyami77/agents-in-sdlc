---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Agent mode を確認します。Plan でアプローチを決め、Autopilot で issue から filtering を実装し、Interactive mode で結果をレビューして検証します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

最初に、小さな機能を project に追加しました。しかし、より大きな変更には、より確実な process が必要です。GitHub Copilot app は組織の既存 workflow に沿って作業し、適切なものを適切な方法で構築できるようにします。このレッスンから数回にわたり、一般的な agent 主導の開発 process を進めます。Issue を起点に新機能を作成し、コードが有効で、機能が期待どおりに動作することを確かめ、最終的に project にマージします。

> [!NOTE]
> 機能の workflow を続ける間、同じ session を使います。通常は作業対象の file type ごとに異なる session や PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい agent session を開始します。
- **Plan** mode で要件を定義します。
- **Autopilot** mode を使って新機能を実装します。
- コードをレビューします。
- browser canvas で機能を手動で検証します。

この機能の作業を続けながら、repository instructions を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加し、QA agent を作成して、feature PR を開きます。

## シナリオ

Tailspin Toys の catalog が増え、利用者は category と publisher でゲームを絞り込む必要があります。backlog issue には機能が記載されていますが、category の組み合わせ方など、実装前に合意する詳細があります。Plan mode で決定し、範囲を明確にした実装を Autopilot に許可します。

## 背景

AI coding agent を開発 workflow に導入しても、基本は変わりません。むしろ、基本はさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容の詳細が記された issue を開きます。
2. 構築する内容を計画します。
3. コードを構築してレビューします。
4. test を実行してコードを検証します。
5. 新機能を手動で検証します。
6. pull request（PR）を作成します。
7. コードがレビューされ、continuous integration process が成功したら、コードをマージします。

> [!NOTE]
> 具体的な内容はチームや組織によって異なりますが、通常は上記の流れを調整したものになります。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じレビュー process を通るようにできます。

## Session mode

**Session mode** は、agent がどの程度自律して動くかを制御します。prompt field の下にある dropdown から設定し、いつでも変更できます。

- **Interactive**: ユーザーと agent が協力して作業します。agent は変更を提案し、次に進む前に入力を待ちます。
- **Plan**: agent が最初に計画を作成します。実行前に計画をレビューして承認します。
- **Autopilot**: agent は完全に自律して作業し、入力を待たずにコードを書き、test を実行して反復します。

Plan mode で開始し、計画をレビューしてから、Autopilot で実装します。

## Issue からセッションを開始する

開始前に、star-rating PR がマージされ、ローカルの `main` が最新であることを確認します。

1. sidebar の **Sessions** で Tailspin Toys repository folder を選択し、repository 上部の navigation から **Issues** を選択します。既存の **Allow users to filter games by category and publisher** issue を開きます。この issue は template に用意されているため、作り直す必要はありません。
2. **New session** を選択し、更新済みの `main` に基づく **new working tree** を選びます。

    ![GitHub Copilot app の Issue view で、New session button を矢印で示した画面](../../../_images/app-new-session-from-issue.png)

3. Issue が session に添付されていることを確認し、mode selector から **Plan** を選択します。

> [!NOTE]
> Node.js または npm が見つからない場合は、agent に `node --version` と `npm --version` を実行するよう依頼します。terminal では実行できるのに session で失敗する場合は、app の PATH または version manager の設定を確認し、app を完全に終了してから再度開きます。失敗した操作と check を再試行してください。agent が見つけられないという理由だけで npm を再インストールしないでください。

## Filtering 機能を計画する

計画を立てると、Copilot がコードを書く前にアプローチを確認できます。issue から開始したため、Copilot は機能 request をすでに context として持っています。次を送信します。

```plaintext
Build this feature.
```

Copilot の質問に回答し、issue の acceptance criteria と計画を比較します。category と publisher の filtering、accessible な control、data-access の変更、test が含まれていることを確認します。複数 category の組み合わせや、該当するゲームがない場合の動作など、不明確な内容について話し合います。

計画には、project 既存の tool を使った lint、unit test、E2E test、type check を含めます。filtering の実装と test に範囲を絞ってください。quality workflow を完了してから PR を作成します。計画は承認前に修正を依頼し、後の検証に使えるよう issue URL と合意事項を控えておきます。

## Autopilot を明示的に承認する

計画に納得したら、**Approve and implement this plan**、または使用中の version で同等の選択肢を選びます。承認しただけで mode が切り替わると思い込まず、現在の mode を確認してください。mode indicator が **Autopilot** と表示されていることを確かめます。

Copilot が実装を開始します。確定した計画に沿ってコードを生成し、test を実行しながら作業を繰り返します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があるため、先に計画をレビューしてください。依存関係が不足している、または port が競合していると報告された場合は、check を完了と判断する前にセットアップの問題を解決します。停止してよいのは、自分で起動した server のみです。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめましょう。

1. **Changes** を開き、filtering の実装と test を確認します。
2. 複数 category と publisher の組み合わせも含め、結果を issue と承認した追加要件に照らし合わせます。変更が既存の repository instructions に従っていることも確認します。
3. lint、unit test、E2E test、type check の結果を調べます。skip された check は成功ではありません。
4. 実装を承認する前に失敗を解消し、影響を受ける check を再実行します。Playwright の E2E 設定は preview を build して配信し、ローカル server を再利用する場合があります。test 対象の server が以前のレッスンではなく、この worktree のものか確認してください。

## 新機能を試す

コードに問題がないように見えても、実際に動くか確認しましょう。前と同じように app を起動し、browser canvas で site を開きます。

1. 次の prompt で、app を起動して browser canvas で page を開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくすると app が起動し、Copilot app 内に browser window が開きます。
3. category を選択し、結果が一致することを確認します。filter を解除し、次に publisher を選んで結果を確認します。
4. category と publisher の filter を組み合わせ、issue と承認済みの計画に沿う結果か確認します。複数 category を使う場合は、合意した組み合わせ動作を確かめます。
5. 結果が 0 件になる組み合わせを試し、empty state を確認します。filter を解除または reset し、全件が再表示されることを確認します。
6. 確認が終わったら、次の prompt で、この session で起動した dev server を停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 次のレッスンのためにこの session を保持する

filtering の PR はまだ作成またはマージしないでください。レッスン 4 から 8 まで、この session、worktree、branch を使い続けます。次は custom instructions を filtering code に適用し、skill と QA の改善を追加してから、レッスン 8 で PR を作成してマージします。dev server を停止した後は session を開いたままにし、次のレッスンで同じ session に戻ってください。

## まとめと次のステップ

複数の agent mode を使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい agent session を開始しました。
- **Plan** mode で要件を定義しました。
- **Autopilot** mode で新機能を実装しました。
- コードをレビューしました。
- browser canvas で機能を手動で検証しました。

次は、文書化された慣例に沿うように code generation を行うため、[custom instructions を使います][next-lesson]。

## リソース

- [GitHub Copilot app で agent session を使う][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
