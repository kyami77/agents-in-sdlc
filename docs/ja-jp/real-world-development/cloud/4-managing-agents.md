---
title: "演習 4 - Agent を監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent に、UX の改善と機能の追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期に自律して動作しますが、タスクを監視することも重要です。

cloud agent に割り当てた task を管理する tool は複数あります。たとえば、GitHub.com の [agents page][agents-page] では、open pull request（PR）がある agent task をすべて確認できます。作業内容を調べたり、実行中の session に追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- agents page で cloud agent の task を監視します。
- 実行中の session に指示を追加し、機能を拡張します。

## シナリオ

agent に high-contrast mode の実装を依頼した後、チームは light mode も追加したいと考えました。サイトの style と切り替え機能を更新する作業をすでに進めているため、high contrast に加えて light mode も実装するよう agent に指示します。

## Copilot cloud agent の task を確認する

Copilot cloud agent に割り当てた task の現在の状態を確認しましょう。

1. [Copilot agents page](https://github.com/copilot/agents) に移動します。
2. main pane と左 pane の task の一覧を確認します。割り当てた task として、次の項目が表示されます。
    - codebase のドキュメントを更新する。
    - game を変更するための API を生成する。
    - Web サイトに high-contrast mode を追加する。
3. 実行中の task を 1 つ選択します。Copilot が行った操作を確認します。たとえば次の操作があります。
    - repository を checkout する。
    - Copilot が作業する環境を作成する。
    - MCP server をセットアップする。
    - task を完了するためにさまざまな処理を行う。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される pull request（PR）pane も確認します。PR と変更された file が表示されるため、さらに詳しく確認できます。

## cloud agent の作業に指示を追加する

実行中の task を確認できたので、high-contrast mode の作業中に light mode も追加するよう Copilot に依頼します。

1. high-contrast mode の追加に関する session を選択します。title は Copilot が付けた名前や作業状態によって異なります。

    ![mission control の Accessibility session](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を始めるまで、session を数分確認します。次のような message が表示されると作業を開始したことがわかります。
3. **Steer active session while Copilot is working** dialog に、次の prompt を入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中の agent task と steer active session dialog が強調表示された agents page の screenshot](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が prompt を受け取り、作業に追加する様子を確認します。

## Copilot に作業を進めてもらう

これまでと同様に、Copilot は更新された task に取りかかります。message を送信した時点で作業中の手順が終わると、新しい request が workflow に追加されます。

今回も数分かかります。待っている間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent task を監視し、案内する中心的な場所である Copilot agents page を確認しました。mission control を使うと、実行中と完了済みの task を追跡し、作業内容を確認し、実行中の task の方向を変えられます。

次のことを行いました。

- mission control と agents page で cloud agent task を監視しました。
- 実行中の session に指示を追加して機能の拡張を依頼しました。

Copilot が accessibility 機能を実装する間に、次は Copilot が作成した [pull request を反復して改善します][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agents][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
