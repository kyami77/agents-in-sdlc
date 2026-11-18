---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有の backlog を使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を体験します。変更の計画、コードの作成、動作のテスト、pull request のレビュー、agent を使った作業管理を、反復しながら進めます。

## シナリオ

架空の会社 Tailspin Toys で新しく開発者として働きます。Tailspin Toys は、開発者をテーマにしたボードゲーム向けのクラウドファンディングを提供しています。チームの backlog は GitHub Issue として作成済みです。filtering や pagination などの機能開発と、accessibility や coding standard などの品質改善が含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。作りたいものに合った環境を選んでください。各ワークショップでは独自のセットアップを行い、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

terminal の agent 型 assistant を使います。Plan と Autopilot mode、instructions、skill、custom agent、Playwright MCP、Agent Merge、実用的な slash command を確認します。

### [GitHub Copilot app][app]

desktop app で agent session を並行して実行し、session mode を切り替え、Canvas で共同作業し、GitHub issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期 agent に割り当て、作業を案内して進行状況を監視し、作成された pull request を確認します。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。editor を離れずに、Copilot Chat agent mode、MCP server、custom agent を利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
