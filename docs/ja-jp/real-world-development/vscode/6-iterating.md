---
title: "演習 6 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験を改善するいくつかのタスクに GitHub Copilot と取り組んできました。agent mode で client と server にまたがるフィルター機能を追加し、Playwright MCP server を使って browser で手動テストし、さらに custom agent で accessibility 機能（high-contrast と light-mode の toggle）を実装しました。作業中の session に指示を追加し、変更を広げる方法も学びました。次はローカルの変更を公開し、チームと同じようにレビューします。

### シナリオ

生成 AI を導入しても、software design と DevOps の基本は変わりません。Copilot が作成したものには、引き続き実際の review process が必要です。Codespace から accessibility の変更を push し、pull request を開いて、ほかのチームメンバーに共有する前に diff を確認しましょう。

## アクセシビリティ機能を公開する

[演習 4][exercise-4] と [演習 5][exercise-5] で accessibility custom agent を使って実装した high-contrast と light-mode の toggle は、Codespace に commit 済みの変更としてあります。branch に push して pull request を開き、チームにレビューしてもらいましょう。

1. Codespace に戻ります。
2. VS Code で **Source Control** view を開きます。
3. accessibility の変更が commit 済みであることを確認します。演習 5 の未 commit 変更がある場合は、ここで stage し、`Add high-contrast and light-mode toggles` などの内容が分かるメッセージで commit します。
4. **Publish Branch** を選択して branch を公開します（または **...** menu → **Push** を使います）。
5. VS Code から github.com で新しい branch を開くように案内されたら、その prompt を受け入れます。または、repository に移動し、branch banner で **Compare & pull request** を選択します。
6. 分かりやすい title（例: `Add high-contrast and light-mode toggles`）と、実施した内容と理由をまとめた簡潔な説明を設定します。
7. **Create pull request** を選択します。
8. PR を開いたら、**Files changed** tab を選択し、作業全体を確認します。特に次の点を確認してください。
   - mode を切り替える UI component。
   - 利用者の設定を保持する local storage の使用。
   - high-contrast と light mode の CSS または style の変更。
   - accessibility attribute（ARIA label、keyboard navigation など）。
   - mode 切り替えを管理する JavaScript/TypeScript code。
9. **Conversation** tab に戻ります。
10. workflow の承認待ちがある場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. workflow が完了するまで待ちます。問題がなければ、すべて成功します。

> [!TIP]
> accessibility の変更について別の意見が必要ですか。PR comment で `@copilot` にタグを付け、「この PR に追加の WCAG の問題がないかレビューしてください」や「keyboard navigation の改善を提案してください」などと依頼します。Copilot は comment に対応する新しい session を開始します。

## 省略可能な演習 - ローカルでさらに試す

IDE で agent と反復的に作業することもスキルの 1 つです。身につけるには繰り返しが必要です。VS Code から始められる追加 session の例を紹介します。

- ゲーム詳細ページに支援者向けの関心フォームを追加します。
- ゲーム一覧ページに pagination を実装します。
- `src/lib/` の data-access helper に入力検証と error handling を追加します。
- accessibility agent の対象範囲を広げ、たとえばサイト全体の keyboard focus 順序を監査します。

## まとめ

おめでとうございます。VS Code harness を完了しました。このラボでは、次のことを行いました。

- **Playwright MCP で機能を手動テストしました。** Playwright MCP server を追加し、PR を開く前に Copilot に browser を操作させてフィルタリング機能を検証しました。
- **agent mode で stack 全体にわたる変更を調整しました。** 1 つの session で client、server、test にまたがるフィルタリング機能を追加しました。
- **custom agent を使いました。** agent picker から accessibility に特化した custom agent を選び、repository に対して high-contrast mode を実装する様子を確認しました。
- **agent session を管理し、方向付けしました。** 提案された変更を inline で確認して必要なものを採用し、light-mode の follow-up で session を拡張しました。
- **pull request で一連の作業を完了しました。** ローカルの変更を公開し、チームと同じように全体をレビューしました。

## 振り返りと次のステップ

これで必須の VS Code harness は完了です。ワークショップはここで終了できます。

Copilot agent の機能についてさらに学ぶ場合は、ほかの harness で異なる surface から関連するシナリオに取り組めます。

- 💻 **[CLI harness](../../cli/)** — terminal から 2 つの変更をレビューして公開します。Plan と Autopilot mode、skill、custom QA agent、Playwright MCP、Agent Merge、context を保った handoff を扱います。
- ☁️ **[Cloud agent harness](../../cloud/)** — Issue を cloud agent に割り当て、agents page で session を監視し、pull request 上で非同期に反復することに重点を置きます。

ここで始めた作業をさらに発展させることもできます。[awesome-copilot][awesome-copilot] には、自分の project に応用できる instruction file、custom agent、skill が多数あります。

省略可能な拡張演習の[オプション: Foundry を組み込む][exercise-7]では、VS Code と Microsoft Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
