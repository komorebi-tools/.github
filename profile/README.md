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

## はじめかた

### 1. ソフトウェアのインストール

各ツールを動かすために、PC に以下をインストールしておく必要があります。

| ソフトウェア | 使うツール |
|---|---|
| Node.js | Report Studio, Slide Editor, PPTX Skill |
| Python | Slide Editor, FA Analysis Skill, ML Summary Bot |

> SprintSignal は Google Apps Script で動くため、ブラウザだけあれば OK です。

**Mac での簡単なインストール方法 (ターミナルに 1 行ずつコピペ):**

まず Homebrew をインストール (初回のみ):
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

次に Node.js と Python をインストール:
```
brew install node python
```

インストールできたか確認:
```
node -v && python3 --version
```
> バージョン番号が表示されれば OK です。

### 2. API キー / 認証情報を受け取る

ツールに応じて、以下の API キーや認証情報を管理者から受け取ってください。

| 種類 | 用途 | 使うツール | 取得方法 |
|---|---|---|---|
| Gemini API キー | 文章整理・図解生成・画像生成など | Report Studio, Slide Editor, PPTX Skill | 社内共通の課金済みキーを管理者から受け取る |
| Google OAuth | Google Slides / Drive へのアップロード | Slide Editor, PPTX Skill | 各ツールの README を参照 |
| Anthropic API キー | メール要約 (Claude) | ML Summary Bot | 管理者から受け取る |
| Slack Bot Token | Slack への自動投稿 | ML Summary Bot | 管理者から受け取る |
| Slack Incoming Webhook URL | Slack への自動投稿 | SprintSignal | 管理者から受け取る |
| Gmail OAuth | メール取得 | ML Summary Bot | 各ツールの README を参照 |

### 3. ツールをダウンロードして起動

各ツールの README にセットアップ手順を記載しています。基本的な流れ:

1. ターミナルで `git clone <リポジトリ URL>` を実行してソースコードをダウンロード
2. 依存パッケージをインストール (`npm install` や `pip install` など)
3. `.env` ファイルに API キーを設定
4. 起動
