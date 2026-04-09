# Komorebi Tools

コモレビ社内ツールのソースコードを管理・共同編集するための GitHub Organization です。

## ツール一覧

| ツール | 概要 | 管理者 |
|---|---|---|
| [Report Studio](https://github.com/komorebi-tools/report-studio) | テキスト原稿から図解付き Web レポート / PDF を自動生成 | 安原 |
| [Slide Editor](https://github.com/komorebi-tools/slide-editor) | Markdown / テキストから PPTX スライドを自動生成 | 安原 |
| [PPTX Skill](https://github.com/komorebi-tools/pptx-skill) | PowerPoint スライド生成エンジン (Claude Code Skill) | 安原 |
| [FA Analysis Skill](https://github.com/komorebi-tools/fa-analysis-skill) | FA データ分析スキル | 安原 |
| [SprintSignal](https://github.com/komorebi-tools/sprint-signal) | スプシ「コンサル_プロジェクト管理」の更新を GAS で検知し、要約を Slack に自動投稿 | 佐々木 |
| [ML Summary Bot](https://github.com/komorebi-tools/ml-summary-bot) | consulting-team ML に届いたメールを要約し、Slack に自動投稿 | 佐々木 |

## セットアップ

各ツールの README にセットアップ手順を記載しています。基本的な流れ:

1. リポジトリを clone
2. 依存パッケージをインストール
3. `.env` に API キーを設定
4. 起動
