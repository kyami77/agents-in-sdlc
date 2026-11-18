---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys project 用の Node.js をインストールし、template から repository のコピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub を一元的に扱う desktop app です。Issue と pull request にすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、自分の computer で Astro 製の Tailspin Toys app を更新します。始める前に、ローカル環境に Node.js をインストールし、その後 Copilot app をインストールします。

このレッスンでは、次のことを行います。

- project の test を自分の computer で実行できるように Node.js をインストールします。
- template から Tailspin Toys project のコピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、agent に機能を実装させ、Tailspin Toys の test suite をローカルで実行します。そのためには、project に必要な唯一の runtime である **[Node.js][nodejs]** が必要です。現在の **LTS** release をインストールしてください。

どの platform でも、公式 installer を使うのが最も簡単です。

1. Windows Terminal、macOS Terminal、または普段使用している terminal を開きます。
2. 次の command を実行して、インストール済みの Node.js version を確認します。

    ```shell
    node --version
    npm --version
    ```

3. 両方の command が成功し、Node.js version が project の README と `package.json` に記載された要件を満たしている場合は、次の section に進みます。

> [!TIP]
> Node.js が未インストールの場合、または更新が必要な場合にのみ、これらの手順を実行してください。

4. [Node.js の download page][node-download] を開きます。
5. 使用中の OS 向けの **LTS** build を download します。
6. installer を実行し、既定の設定を選びます。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しい terminal window を開きます。
8. 新しい terminal で次の command を実行して、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストール済みの Node.js と npm の version が表示されることを確認します。

> [!IMPORTANT]
> Copilot app の agent からも `node --version` と `npm --version` を実行できる必要があります。普段使う terminal で成功しても、app から使えるとは限りません。Node.js のインストール後、または PATH を変更した後は、app を完全に終了してから再度開いてください。session 内でどちらかの command が引き続き失敗する場合は、先に[session 環境のトラブルシューティング手順][session-environment]を実施してください。

> [!IMPORTANT]
> E2E check を実行するには、各 worktree に project dependency と Playwright Chromium も必要です。worktree を準備する際は Tailspin Toys repository の README に従い、インストールの request は内容を確認してから承認してください。

## lab repository をセットアップする

自分用の Tailspin Toys project のコピーを使います。[template repository][template-repository]から作成してください。新しい repository には lab に必要なすべての file が含まれており、app のインストール時に接続します。

1. 新しい browser window で、この lab の GitHub repository `https://github.com/github-samples/tailspin-toys` に移動します。
2. lab repository のページで **Use this template** button を選択し、次に **Create a new repository** を選択して、自分用の repository を作成します。

    ![Create a new repository が dropdown から選択された Use this template button](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が運営する event の一環としてワークショップに参加している場合は、mentor の案内に従います。それ以外の場合は、GitHub Copilot を利用できる organization に新しい repository を作成します。

    ![github-samples/tailspin-toys を template に指定し、repository 名を入力した Create a new repository form](../../../_images/app-0-create-repository.png)

4. 作成した repository の path（**organization-or-user-name/repository-name**）を控えてください。この後の lab で参照します。

> [!NOTE]
> template から repository を作成すると、GitHub issue の backlog が自動的に作成されます。ワークショップではこれらの issue を使うため、自分で起票する必要はありません。

ワークショップ template の新しいコピーを使ってください。repository instructions、アプリケーション code、test、`quality-checks` skill、既存の Canvas 拡張機能が含まれています。ワークショップ中に skill をカスタマイズし、QA agent を作成します。古いコピーを使用している場合は、必要な file があるか facilitator に確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- 自分の computer で project を build して test するために Node.js をインストールしました。
- template から Tailspin Toys repository のコピーを作成しました。

次は [GitHub Copilot app をインストールし][next-lesson]、作成した repository を接続して workspace を確認します。

## リソース

- [Node.js を download する][node-download]
- [template から repository を作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
