---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、準備が必要です。Tailspin Toys repository のコピーを作成し、[Codespace][codespaces] を起動します。次のレッスンでは、統合 terminal から Copilot CLI をインストールして実行します。

このレッスンでは、次のことを行います。

- template から Tailspin Toys project のコピーを作成します。
- Codespace を作成し、project の準備ができていることを確認します。

## lab repository をセットアップする

自分用の Tailspin Toys project のコピーを使います。[template repository][tailspin-template]から作成してください。新しい repository には lab に必要なすべての file が含まれます。

1. 新しい browser window で [Tailspin Toys template][tailspin-template] に移動します。
2. **Use this template** を選択し、次に **Create a new repository** を選択して repository のコピーを作成します。
3. GitHub または Microsoft が運営する event の一環としてワークショップに参加している場合は、mentor の案内に従います。それ以外の場合は、GitHub Copilot を利用できる organization に repository を作成します。
4. 後で参照するため、作成した repository の path（`organization-or-user-name/repository-name`）を控えてください。

> [!NOTE]
> template から repository を作成すると、GitHub issue の backlog が自動的に作成されます。ワークショップではこの issue を使います。自分で issue を作成する必要はありません。

ワークショップ template の新しいコピーを使ってください。必要な repository instructions、アプリケーション code、test、`quality-checks` skill、backlog が含まれています。古いコピーを使用している場合は、必要な file があるか facilitator に確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は cloud 上の開発環境です。browser からコードを書き、実行し、debug できます。複数の programming language、extension、tool に対応した、機能豊富な editor を利用できます。

1. 作成した repository に移動します。
2. **Code** を選択します。
3. **Codespaces** tab を選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。template が project dependency、Playwright Chromium、ローカル database をインストールします。
5. **Do you trust the authors of the files in this folder?** と表示されたら、**Trust Folder & Continue** を選択します。
6. repository の root で terminal を開き、app を起動します。

   ```bash
   npm run dev
   ```

7. Codespaces で port `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. terminal に戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発 server を停止します。

## まとめと次のステップ

セットアップが完了しました。このレッスンでは、次のことを行いました。

- template から Tailspin Toys project のコピーを作成しました。
- Codespace を作成し、project の準備ができていることを確認しました。

次は Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub account で認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template から repository を作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
