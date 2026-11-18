---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、準備が必要です。Tailspin Toys repository のコピーを作成し、作業用の [Codespace][codespaces] を起動します。また、editor で GitHub Copilot Chat が動作することを確認します。

## lab repository をセットアップする

作成するコードのための repository を用意するには、[template][template-repository]から新しい instance を作成します。新しい instance には、lab に必要なすべての file が含まれます。演習中は、この repository を使います。

1. 新しい browser window で [Tailspin Toys template][tailspin-template] に移動します。
2. lab repository のページで **Use this template** button を選択します。次に **Create a new repository** を選択して、自分用の repository を作成します。

    ![テンプレートを使用 button](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が運営する event の一環としてワークショップに参加している場合は、mentor の案内に従います。それ以外の場合は、GitHub Copilot を利用できる organization に新しい repository を作成します。

    ![repository template の設定を入力する](../../../_images/ex0-repository-settings.png)

4. 作成した repository の path（**organization-or-user-name/repository-name**）を控えてください。この後の lab で参照します。

> [!NOTE]
> **backlog の準備ができています**
>
> template から repository を作成すると、GitHub issue の backlog が自動で作成されます。ワークショップではこの issue を使います。自分で issue を作成する必要はありません。

## Codespace を作成する

次に、Codespace を使って lab の演習を行います。

[GitHub Codespaces][codespaces] は cloud 上の開発環境です。browser からコードを書き、実行し、debug できます。複数の programming language、extension、tool に対応した、機能豊富な IDE を利用できます。

1. 新しく作成した repository に移動します。
2. 緑色の **Code** button を選択します。

    ![Code button を選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** tab を選択し、**+** button を選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、service を手動でインストールするよりもはるかに短時間で済みます。作成中に GitHub Copilot のほかの機能を確認してみましょう。この後でそれらを使います。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。今は browser の tab で開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作します。どちらも、スムーズに進めるために必要な前提条件がすべてインストールされた環境を提供します。ローカルで作業する場合は、clone した repository を VS Code で開き、prompt が表示されたら **コンテナーで再度開く**を選択してください。VS Code は Codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat agent mode を使うには、IDE に GitHub Copilot Chat extension がインストールされている必要があります。GitHub Codespace を使っている場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat extension がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions view を開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

extension をインストールした後、GitHub account で認証が必要になる場合があります。

1. Codespace に戻ります。
2. editor の右側に Copilot Chat が表示されていない場合は、Codespace 上部にある **Copilot Chat** icon を選択します。
3. Copilot Chat window に「Hello world」のような message を入力して <kbd>Enter</kbd> を押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub account での sign in を求められます。画面の案内に従って認証します。

    ![Copilot Chat の認証 prompt の例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat window が表示されます。

## まとめ

lab repository のコピーを作成できました。また、コードを書くときに使う Codespace の作成を始めました。

## 次のステップ

Copilot を使って作業を始めましょう。[演習 1 - Custom instructions][next-lesson]に進み、Copilot にプロジェクトの慣例を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template から repository を作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
