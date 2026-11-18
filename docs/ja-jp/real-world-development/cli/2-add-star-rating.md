---
title: "レッスン 2 - star rating を追加する: 小さく始める"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送された browser で確認し、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに初めて変更を加えます。小さな変更から始めましょう。ゲームのデータにはすでに star rating がありますが、ホームページのゲームカードにはまだ表示されていません。agent に表示させ、変更を確認して、最初の pull request としてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を始めます。
- agent にプロジェクトの小さな変更を依頼します。
- `/diff` で変更を確認します。
- app を実行し、転送された browser で変更を確かめます。
- 最初の pull request を開いてマージします。

## シナリオ

Tailspin Toys の各ゲームには star rating があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリー、publisher、説明しか表示されていません。最初の作業として、agent に既存の rating を各カードに表示させます。これは小規模で独立した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**は、タスクについて Copilot CLI と作業する場所です。Copilot app とは異なり、通常の CLI 会話では、専用の worktree を作るのではなく、terminal で現在 checkout しているリポジトリと Git branch を使います。保存した会話に戻ることもできます。ファイルと branch は、通常の Git の状態としてディスク上に残ります。

会話では、prompt と agent の応答、ファイルを調査・編集する際の agent の tool 操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

新しい会話を始めて、機能を実装しましょう。

1. Codespace に戻ります。
2. まだ terminal を開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を使い、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次の prompt で変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを確認し、ゲーム詳細の表示に使われているファイルを特定して、必要なコードを作成します。Copilot CLI で新しい機能を追加できました。

## diff を確認する

小さな変更でも、AI が生成した変更はマージする前に確認してください。Copilot CLI の画面で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. rating がある場合は各ゲームカードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. 両方の状態をテストで確認していることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. 確認が終わったら、<kbd>Esc</kbd> を押して diff 画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI tool は決定論的ではないため、実際のコードはこの例と異なる場合があります。特定の実装に一致することではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動させ、Codespaces で転送された browser から新しい UI を確認します。

1. Copilot に app の起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces で port `4321` が利用可能になったら、**ブラウザーで開く**を選択します。
3. ゲームカードに 5 段階の rating が表示されることを確認します。
4. Copilot に戻り、起動した server の停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を開いてマージする

機能を作成できました。新しいコードをプロジェクトにマージするため、pull request（PR）を作成しましょう。

1. default agent に変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 準備ができたら **Pull request をマージ**を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、初めての変更をリリースできました。次のことを行いました。

- 機能ブランチで、目的を絞った Copilot の会話を始めました。
- agent にゲームカードの小さな変更を依頼しました。
- `/diff` で変更を確認しました。
- app を実行し、転送された browser で star rating を確認しました。
- 最初の pull request を開いてマージしました。

次は、[フィルタリング issue から始め、Plan と Autopilot mode を使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
