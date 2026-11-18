---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue からフィルタリング機能を実装し、Interactive mode で結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。しかし、より大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の workflow に沿って作業できるため、適切な方法で適切なものを構築できます。ここからは、agent を活用した標準的な開発プロセスをいくつかのレッスンで進めます。issue を基に機能を作成し、コードが正しいこと、機能が期待どおりに動作することを確かめ、最後にプロジェクトへマージします。

> [!NOTE]
> 機能の workflow を続ける間、同じ会話と branch を使います。通常は、扱うファイルの種類ごとに異なる branch や PR を使いますが、ここでは主な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の新しい会話を始めます。
- Plan mode で要件を定義します。
- Autopilot mode を使って新しい機能を実装します。
- コードを確認します。
- 転送された browser で機能を手動で検証します。

この後のレッスンでは、リポジトリの instructions を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加し、QA agent を作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のゲーム一覧が増えてきたため、利用者がカテゴリーと publisher でゲームを絞り込めるようにします。backlog の issue には機能が記載されていますが、カテゴリーを組み合わせる方法など、実装前に合意する必要がある詳細もあります。Plan mode でそれらを決めてから、範囲を明確にした実装を Autopilot に許可します。

## 背景

AI coding agent を開発 workflow に導入しても、基本は変わりません。むしろ、基本の重要性はさらに高まります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記述した issue から始めます。
2. 構築する内容の計画を立てます。
3. コードを構築して確認します。
4. test を実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. pull request（PR）を作成します。
7. コードがレビューされ、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、通常は上記の流れを調整したものになります。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じ審査プロセスを通るようにできます。

## 会話 mode

**会話 mode** は、agent がどの程度自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、mode を切り替えられます。

- **Interactive:** 開発者と agent が協力して作業します。agent は変更を提案し、次に進む前に入力を待ちます。
- **Plan:** agent は最初に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot:** agent は自律的に作業し、タスクが完了するまでコードを書き、test を実行して、繰り返し改善します。

まず Plan mode で計画を確認し、次に Autopilot で実装します。

## issue から始める

フィルタリングの作業を始める前に、Codespace と terminal の準備を確認します。

1. Codespace に戻ります。停止している場合は、再開してから続けます。
2. star-rating PR がマージされていることを確認します。
3. terminal を開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
4. `main` を更新し、フィルタリング用の branch を作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** tab を開きます。
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue を prompt に追加し、**Session** tab に戻ります。

prompt の先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の issue または pull request（PR）をコンテキストに含められます。

## フィルタリング機能を計画する

計画を立てることで、機能の実装やタスクの実行方法を Copilot に依頼する前に定義できます。複雑な作業では、時間をかけて計画することをお勧めします。Plan mode に切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。prompt の下にある mode indicator に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の参照に続けて、次の prompt を入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot が計画を作成します。まずプロジェクトを調べ、最適な方法を検討します。

3. フィルタリングの動作について質問された場合は、希望に応じて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 自分の決定と一致しない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成して確認したので、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot を使うと、Copilot は完了したと判断するまで問題に取り組み、反復的に改善します。

1. **計画を受け入れて Autopilot で構築（推奨）**、またはインストール済みの version で同様の名前が付いた選択肢を選びます。
2. prompt の下にある mode indicator が **Autopilot** と表示されていることを確認します。
3. Copilot が確定した計画に沿って作業し、コードを生成して test を実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、または port が競合していると Copilot が報告した場合は、チェックを完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードを生成した後、ほかのコードと同様にマージする前にレビューします。コードを確認し、サイトを実行して、すべてが正しく見えることを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。mode indicator に **Plan** または **Autopilot** と表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装と test を確認します。
3. issue と計画中に決めた内容を基に、結果を確認します。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して diff view を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がなさそうでも、実行できることを確かめましょう。前と同じように app を起動し、Codespaces で転送された port からサイトを開きます。

1. Copilot に app の起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces で port `4321` が利用可能になったら、**ブラウザーで開く**を選択します。
3. 計画で合意したカテゴリーと publisher のフィルタリング、およびその組み合わせを試します。
4. リセットと検索結果がない場合の動作が、issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発 server を停止するよう Copilot に依頼します。

## まとめと次のステップ

会話の mode を切り替えて、機能を構築しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の新しい会話を始めました。
- Plan mode で要件を定義しました。
- Autopilot mode を使って新しい機能を実装しました。
- コードをレビューしました。
- 転送された browser で機能を手動で検証しました。

次は、[custom instructions を使って][next-lesson]、コードが文書化された慣例に沿うようにする方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
