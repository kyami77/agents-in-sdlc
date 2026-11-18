---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages workflow を構成し、pull request を確認して、公開された Space Quiz を検証します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開し、誰でもアクセスできる場所に配置します。Copilot は workflow の準備とデプロイの案内を行いますが、変更を確認し、サイトが公開されたことを確かめるのは開発者の役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイ構成を依頼します。
- workflow と pull request を確認してからマージします。
- デプロイの進行を追い、公開されたサイトを検証します。

## シナリオ

Space Quiz は GitHub リポジトリにありますが、まだ誰も簡単に試せる場所がありません。GitHub Pages を使って単一ファイルのサイトを公開すると、サーバーやランタイム依存関係を追加せずに、公開 URL からアクセスできるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> pull request の作成により、コードが commit および push される場合があります。この prompt を送信する前に、承認設定と送信先のリポジトリを確認してください。**すべてを承認**に設定している場合、個別の確認 prompt が表示されるとは限りません。

`space-quiz` の Copilot app セッションで、次の prompt を送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## workflow と pull request を確認する

1. README が、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. workflow のビルドとデプロイの手順、トリガー、権限を含めて、pull request の変更を確認します。
3. workflow が、マージされていない pull request ではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新された差分を確認します。マージを許可する前に、必要な pull request チェックがすべて成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次の prompt を送信すると、レビュー済み pull request のマージ、リポジトリの Pages 設定変更、サイトの公開を agent に許可します。レビューを完了し、リポジトリと pull request を確認してから送信してください。承認設定によっては、これらの操作に別の確認 prompt が表示されない場合があります。

`<PR_NUMBER>` を確認した pull request の番号に置き換えてから、次の別個の prompt を送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済み space-quiz pull request、Build website、Upload artifact、Deploy to Pages の各 step が成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを検証する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

agent が Pages の有効化や pull request のマージを行えない場合は、案内された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまでは、作業が完了したと見なさないでください。

## まとめと次のステップ

Space Quiz が公開され、agent の報告だけに頼らず、デプロイを検証できました。次は[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
