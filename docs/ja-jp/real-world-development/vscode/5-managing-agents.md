---
title: "演習 5 - Agent を監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot を agent mode で使うと、codebase を調べ、変更を提案し、file を編集し、command を実行します。作業は自分の computer 上でリアルタイムに進むため、VS Code の Copilot Chat には各 tool call と file の編集が表示されます。diff を確認して変更ごとに採用または却下し、editor から離れずに follow-up prompt を送って作業を改善したり拡張したりできます。

この演習では、次のことを行います。

- accessibility agent の作業をリアルタイムで監視します。
- 提案された file の編集を確認して採用します。
- 実行中の session に light-mode toggle を追加するよう指示します。

## シナリオ

[演習 4][previous-lesson] で使った accessibility custom agent は、high-contrast mode を実装中です。作業中に design team から追加の request が届きました。既定の theme が暗すぎると感じる利用者向けに、light-mode toggle も追加したいとのことです。新しい会話を始めるのではなく、既存の session に追加の指示を送ります。

## Copilot の進行状況を監視する

Copilot Chat view には、agent が実行する各 step（調べる file、実行する command、提案する編集）が表示されます。file の編集はそれぞれ diff と **Keep** / **Undo** control とともに chat に表示されます。

1. 演習 4 で使った Copilot Chat view に戻ります。
2. agent の作業中に chat をスクロールして、tool call を確認します。file の読み取り、編集、実行を求める terminal command が表示されます。
3. agent が file の編集を提案すると、diff が chat 内に表示されます。
    - **Keep** を選択して変更を採用します。
    - **Undo** を選択して変更を元に戻し、agent に別の方法を試させます。
4. chat 上部の **Files changed** indicator を開くと、この session で agent が変更した file 全体を確認できます。

> [!NOTE]
> terminal command の実行など、tool call に許可が必要な場合、agent は一時停止して **Allow** prompt を表示します。自分が許可できる command のみを承認してください。

5. agent が high-contrast mode の実装を終えるまで待ちます。最後に、変更内容の概要が表示されます。

## Agent に light mode の追加を依頼する

high-contrast mode ができたので、同じ会話で light-mode toggle も追加します。同じ chat session を続けると、agent は以前の作業（変更した component、style、設定を local storage に保持する方法など）のコンテキストを使えます。

1. Copilot Chat の入力欄に、次の follow-up prompt を送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent は前に行った変更を確認して、light mode を追加します。
3. high-contrast のときと同じように、新しい編集を確認して **Keep** または **Undo** を選択します。
4. 望まない方向に進んだ場合（例: toggle UI の意図しない refactor）、chat の入力欄の横にある **Stop** button で停止し、明確な follow-up message を送ります。

> [!TIP]
> 作業中の session に指示を追加すると、最初からやり直すより速く調整できます。follow-up prompt で「CSS を簡潔にして、selector の複製ではなく CSS variable を使う」「3 つ目の toggle を追加せず、light mode を既存の設定 panel に統合する」などと方向を修正できます。また、「両方の toggle に keyboard shortcut を追加する」と範囲を広げることもできます。

## 作業内容を確認する

commit する前に、agent が変更した内容を確認します。

1. VS Code で **Source Control** view を開きます。
2. 変更された file の一覧を確認します。Astro component、style、関連 test などが更新されています。
3. 変更された file をいくつか開き、diff を確認します。custom agent の accessibility 指針（ARIA attribute、keyboard navigation、semantic HTML、local storage による設定の保持など）が反映されていることを確認します。
4. 問題がなければ **Source Control** panel で変更を stage して commit します。次のレッスンで変更を公開します。

## まとめと次のステップ

Copilot Chat agent mode を使い、実行中の session を監視して方向を示しました。file の編集を確認し、採用または却下し、コンテキストを保ったまま follow-up prompt を送って作業を拡張しました。

このレッスンでは、次のことを行いました。

- agent の作業をリアルタイムで監視しました。
- file の編集ごとに採用または却下しました。
- 作業中の session に follow-up prompt を送り、方向を調整しました。

次の[レッスン][next-lesson]では accessibility の変更を pull request として公開し、この lab で local custom agent が生成した変更を確認します。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: Custom agents][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
