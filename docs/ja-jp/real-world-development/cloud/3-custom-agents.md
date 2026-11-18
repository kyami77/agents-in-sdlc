---
title: "演習 3 - Custom agents"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の[custom agent][custom-agents-concept]を使うと、開発 workflow の特定のタスクや分野に合わせた、専門的な AI assistant を作成できます。repository の `.github/agents` folder に Markdown file を作り、対象の作業を効果的に進めるための指示、ベスト プラクティス、coding pattern、専門知識を Copilot に提供します。チームの知識を再利用可能な agent にまとめることもできます。たとえば、[WCAG][wcag] 準拠をチェックする accessibility agent、secure coding practice に従う security agent、一貫した test pattern を保つ testing agent などがあります。

custom agent は、プロジェクト内の `.github/agents` folder または global な `~/.copilot/agents` にある Markdown file で定義します。各 file には、少なくとも `name` と `description` を含む YAML frontmatter を記載し、その後に agent の動作、専門性、instructions を定義する Markdown prompt を記述します。

### Custom agent と agent skill の違い

custom agent と [agent skill][agent-skills-concept] には重複する部分があります。どちらも主に Markdown file で定義し、AI が作業を行う方法を指示します。違いを簡潔に表すと、**custom agent は作業者、skill は tool** です。

custom agent は独自のコンテキスト window を持ち、skill や別の agent を調整して作業できます。この lab では、accessibility custom agent が accessibility guideline に照らしてサイトをレビューし、更新します。その作業の一部として、pull request workflow skill や test の実行・管理を行う skill を呼び出す場合があります。

> [!NOTE]
> custom agent の作成に唯一の正解はありません。AI のほかの使い方と同様に、自分の環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

custom agent について、次のことを確認します。

- custom agent の定義方法。
- custom agent へのタスク割り当て。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者が crowdfunding platform を使えるようにしたいと考えています。最近の feedback では、現在の dark theme は、文字と背景の contrast が十分でないため、読みにくいと感じる利用者がいることがわかりました。この accessibility の問題に対応するため、design team は利用者が切り替えられる high-contrast mode の実装を依頼しました。

accessibility は重要なので、できるだけ早く実装する必要があります。custom agent を使って機能を生成します。

## Accessibility custom agent を確認する

accessibility 用の custom agent はすでに作成されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、accessibility custom agent file を確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` field を持つ YAML frontmatter を確認します。

> [!CAUTION]
> custom agent では、`name` と `description` を含む frontmatter が必須です。

3. 続く section を読み、次の内容を確認します。
   - accessible な Web サイト向けコードを生成する際の主な役割。
   - accessibility のベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## issue を作成して割り当てる

Mission control は、環境内のすべての agent を操作する場所です。Copilot cloud agent にタスクを割り当てたり、タスクを監視したり、追加の指示をして方向を変えたりできます。まず high-contrast mode を実装する task を Copilot に割り当てます。

1. repository に移動します。
2. **Issues** tab を選択します。
3. **New issue** を選択して、新しい issue dialog を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て dialog を開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面。custom agent と accessibility が強調表示されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた [GitHub Copilot の custom agent][custom-agents] を確認しました。custom agent を使うと、チームの専門知識や標準を再利用可能な agent にまとめ、特定の作業をより効果的に進めるよう Copilot を導けます。

次のことを確認しました。

- custom agent の定義方法。
- agent にタスクを割り当てる方法。

Copilot が high-contrast mode を実装する間に、次は [mission control から agent の session を監視して方向を示します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [organization で custom agent を使うための準備][org-custom-agents]
- [enterprise で custom agent を使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: Agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
