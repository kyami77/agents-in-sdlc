---
title: "レッスン 2 - star rating を追加する: 小さく始める"
description: "GitHub Copilot app で最初の agent session を開始し、ゲームカードに小さな変更を加えて、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

前のレッスンでは、workspace を確認し、Quick chat を試しました。次は **agent session** を開始し、project に初めての変更を加えます。変更は小さくします。ゲームのデータには star rating がすでにありますが、ホームページの game card には表示されていません。agent に表示を依頼し、変更を確認して、最初の pull request としてマージします。

このレッスンでは、次のことを行います。

- agent session を開始し、その構造を学びます。
- project に小さく焦点を絞った変更を依頼します。
- workspace の差分 view で変更をレビューします。
- app をローカルで実行し、browser で変更を確認します。
- 最初の pull request を開いてマージします。

## シナリオ

Tailspin Toys の各 game には star rating があり、game details page にはすでに表示されています。しかし、ホームページの game card には title、category、publisher、description しか表示されていません。準備運動として、各 card に既存の rating を表示するよう agent に依頼します。小規模で自己完結した変更なので、最初の session に適しています。

## Session の構造

**Session** は、それぞれ独立した workspace で agent と会話する場所です。各 session には**専用の git worktree と branch**が割り当てられます。そのため、一方で機能を追加し、別の session で bug を修正するなど、変更を競合させずに複数の session を同時に実行できます。session は repository ごとに sidebar にまとめて表示されます。session を選ぶと切り替えられます。

session 内には、agent との**会話**、file の調査や編集を行う agent の**tool 操作**、diff を含む**変更された file**一覧が表示されます。

## Session を開始して変更を依頼する

新しい session を開始し、project を調べて機能を実装します。[app のセットアップ][prior-lesson]で、GitHub repository から project を追加しました。その repository 用の新しい session を作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、app を開きます。
2. sidebar 上部の **+** を選択して session を開始します。
3. repository として `tailspin-toys` を選択します。
4. prompt box の下で **new working tree** と **Interactive** mode を選択します。次の prompt を使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して prompt を Copilot に送信します。

Copilot app は最初に独立した project のコピーである新しい worktree を作成し、作業を開始します。次に project を調べ、新機能を追加する file を見つけ、必要なコードを作成します。Copilot app を使って新機能を追加できました。

## diff を確認する

小さな変更でも、AI が生成した変更はマージする前に確認してください。Copilot app で変更を確認しましょう。

1. app の右上にある **Toggle review panel** を選択します。開いた panel で **Changes** を選択し、変更された file とその diff を確認します。

    ![Create PR の右側にある Toggle review panel button を矢印で示した GitHub Copilot app の上部 toolbar](../../../_images/app-2-review-panel.png)

2. game details の表示に使用される中心的な file `GameCard.astro` にコードが追加されていることを確認します。rating がある場合は表示し、`starRating` が `null` の場合は `No rating yet` を表示する、次のような小さな block が追加されているはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI tool は決定論的ではないため、実際のコードは上記の例と異なる場合があります。動作は比較的似ているはずです。

## 変更を確認する

browser を開く前に、agent が実行した自動 check の結果をレビューします。数値の `starRating` と、`null` の場合の代替表示の両方を test していることを確認してください。前提条件が不足していたり check が skip されていたりする場合は成功とは言えません。インストールの request は内容を確認してから承認します。

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを開いてもらい、更新後の UI を確認します。Web サイトを起動し、browser canvas で開くよう依頼します。

> [!TIP]
> Canvas は Copilot app 内で使える interactive widget です。後のレッスンで custom canvas を確認して自分でも作成しますが、ここでは組み込みの browser canvas を使います。

1. 次の prompt を使い、app を起動して browser canvas で page を開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくすると app が起動し、Copilot app 内に browser window が開きます。
3. rating のある game card に 5 点満点の値が表示されることを確認します。
4. 確認が終わったら、次の prompt を使い、この session で起動した dev server を停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初の pull request を作成してマージする

機能を作成できました。新しい code を既存の codebase にマージするため、pull request（PR）を作成します。

1. 右上にある **Create PR** を選択します。
2. prompt が表示された場合は **Sign in with your browser** を選択し、案内に従って認証します。
3. repository policy により、PR の作成前に関連する issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. chat のすぐ上にある **PR** bubble を選択して、review pane で PR を開きます。ここで PR を確認できます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しい dialog window で **Merge pull request** を選択し、pull request をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- agent session を開始し、session の構造を学びました。
- game card に小規模で焦点を絞った変更を行うよう agent に依頼しました。
- workspace の diff view で変更をレビューしました。
- app をローカルで実行し、browser で star rating を確認しました。
- PR 1 を作成し、check をレビューして、明示的にマージしました。

次は、[filtering issue から始めて Plan と Autopilot mode を使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app で agent session を使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue と pull request を管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
