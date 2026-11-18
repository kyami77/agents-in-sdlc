---
title: "演習 4 - Custom agents"
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

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者が crowdfunding platform を使えるようにしたいと考えています。最近の feedback では、現在の dark theme は、文字と背景の contrast が十分でないため、読みにくいと感じる利用者がいることがわかりました。この accessibility の問題に対応するため、design team は利用者が切り替えられる high-contrast mode の実装を依頼しました。

accessibility は重要なので、できるだけ早く実装する必要があります。custom agent を使って機能を生成します。

この演習では、次のことを行います。

- 既存の accessibility custom agent を確認します。
- Copilot Chat で accessibility agent を使い、high-contrast mode を実装します。

## Accessibility custom agent を確認する

accessibility 用の custom agent はすでに作成されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、terminal を開いて `main` から accessibility 作業用の新しい branch に切り替えます（演習 3 のフィルタリング PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` field を持つ YAML frontmatter を確認します。

> [!CAUTION]
> custom agent では、`name` と `description` を含む frontmatter が必須です。

3. 続く section を読み、次の内容を確認します。
   - accessible な Web サイト向けコードを生成する際の主な役割。
   - accessibility のベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## Copilot Chat で custom agent を使う

VS Code は `.github/agents` に定義されたすべての custom agent を Copilot Chat view 下部の agents dropdown に表示します。custom agent を選択すると、その agent の instruction と tool を使って chat session を実行できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択します。各演習で mode と model の選択方法が指定されているので、それに従います。

1. Chat view の agents dropdown で **Agent** が選択されていることを確認します。まだの場合は選択します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

2. chat view 下部の agents dropdown を選択します。既定では、active agent の **default** が表示されています。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. 次の prompt を accessibility agent に送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat は codebase を調査し、変更を提案して project file に適用します。変更内容は file path と diff とともに chat に表示されます。
6. agent が変更した file を示す **Files changed** indicator の内容が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は実際に repository を変更し、作業中に Astro component、CSS、関連 test などの既存 file を編集します。

次の演習では、作業中の変更を確認し、追加の指示をします。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた [GitHub Copilot の custom agent][custom-agents] を確認しました。custom agent を使うと、チームの専門知識や標準を再利用可能な agent にまとめ、特定の作業をより効果的に進めるよう Copilot を導けます。

このレッスンでは、次のことを行いました。

- custom agent の定義方法。
- Copilot Chat agent mode で custom agent を使う方法。

次は[agent の作業を監視して方向を示します][next-lesson]。作業中に変更を確認し、同じ session に light-mode toggle を追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能を test する][previous-lesson] | [次のレッスン: Agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
