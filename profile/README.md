# Komorebi Tools

コモレビ社内ツールのソースコードを管理、共同編集するための GitHub Organization です。

## リポジトリ一覧

| リポジトリ | 分類 | 概要 | 管理者 |
|---|---|---|---|
| [Report Studio](https://github.com/komorebi-tools/report-studio) | Web ツール | テキスト原稿から図解付き Web レポート / PDF を自動生成 | 安原 |
| [Slide Editor](https://github.com/komorebi-tools/slide-editor) | Web ツール | Markdown / テキストから PPTX スライドを自動生成 | 安原 |
| [PPTX Skill](https://github.com/komorebi-tools/pptx-skill) | Claude Code スキル | PowerPoint スライド生成エンジン | 安原 |
| [FA Analysis Skill](https://github.com/komorebi-tools/fa-analysis-skill) | Claude Code スキル | FA データ分析スキル | 安原 |
| [SprintSignal](https://github.com/komorebi-tools/sprint-signal) | 自動化 | スプシ「コンサル_プロジェクト管理」の更新を GAS で検知し、要約を Slack に自動投稿 | 佐々木 |
| [ML Summary Bot](https://github.com/komorebi-tools/ml-summary-bot) | 自動化 | consulting-team ML に届いたメールを要約し、Slack に自動投稿 | 佐々木 |
| [Design System](https://github.com/komorebi-tools/design-system) | 設計基盤 | コモレビのカラー、フォント、コンポーネント定義 + Claude Code スキル | 佐々木 |

## 共通ファイルの仕組み

コモレビのツールは、いくつかの共通ファイルによって品質が統一されています。

Claude Code はセッション開始時にこれらを自動で読み込むため、プロンプトに色やフォントを毎回書く必要がありません。

### プロジェクト共通 (各リポジトリに配置)

| ファイル | 役割 |
|---|---|
| DESIGN.md | 色、フォント、角丸、余白、シャドウを数値で定義。Claude Code はここを見てトークン準拠の CSS を書く |
| SKILL.md | デザイントークン、情報ソース、品質 3 層定義 (L1/L2/L3)、アンチパターン、チェックリスト。Claude Code の判断基準になる |
| contracts/rules.json | 絵文字禁止、#000000 禁止、全角括弧禁止など 9 件の禁止ルール。ファイル編集のたびに自動チェックし、違反があれば警告する |

### 個人の PC 全体 (全プロジェクトに適用)

| ファイル | 役割 |
|---|---|
| CLAUDE.md | 「読んでいないコードは変更するな」「絵文字禁止」「Markdown の改行ルール」など、全プロジェクト共通の作業ルール |
| ~/.claude/settings.json | 権限設定、deny ルール (rm -rf / force push のブロック)、hook 設定、effortLevel など |
| ~/.claude/hooks/design-check.sh | Edit / Write のたびに rules.json の禁止パターンを自動チェックする hook |

### 品質維持の流れ

1. **DESIGN.md** がルールを定義する
2. **SKILL.md** が Claude Code に判断基準を与える
3. **rules.json + hook** が編集のたびに違反を自動検出する

プレビューページで全トークンをビジュアル確認できます → https://komorebi-tools.github.io/design-system/

詳しくは [design-system の README](https://github.com/komorebi-tools/design-system) を参照してください。

---

## Claude Code でツール開発のはじめかた

### 1. ソフトウェアのインストール

各ツールを動かすために、PC に以下をインストールしておく必要があります。

| ソフトウェア | 使うツール |
|---|---|
| Node.js | Report Studio, Slide Editor, PPTX Skill |
| Python | Slide Editor, FA Analysis Skill, ML Summary Bot |

> SprintSignal は Google Apps Script で動くため、ブラウザだけあれば OK です。

Claude Code に以下を伝えてください。

> Homebrew, Node.js, Python がインストールされているか確認して、入っていなければインストールしてください。

### 2. Claude Code の組織共通設定

全メンバーの Claude Code に共通のセキュリティ、品質ルールを適用します。

初回のみ実行してください。

Claude Code に以下を伝えてください。

> komorebi-tools/claude-code-org-setup をクローンして、README の手順に従ってセットアップしてください。

> 詳しいルール内容や FAQ は [Notion の「Claude Code 組織共通設定」ページ](https://www.notion.so/komorebi-inc/Claude-Code-33f13485e9d08044adc8d291c70149dc) を参照してください。

### 3. デザインスキルを追加する

コモレビの UI を作るプロジェクトには、デザインスキルを追加してください。

Claude Code に以下を伝えてください。

> komorebi-tools/design-system リポジトリから .claude/skills/komorebi-design-system/SKILL.md を取得して、このプロジェクトの同じパスに配置して。

これで「コモレビの LP を作って」と伝えるだけで、デザインルールに沿った UI が生成されます。

### 4. API キー、認証情報を用意する

| 種類 | 用途 | 使うツール | 取得方法 |
|---|---|---|---|
| Gemini API キー | 文章整理、図解生成、画像生成、メール要約など | Report Studio, Slide Editor, PPTX Skill, ML Summary Bot | 社内共通の課金済みキーを安原または佐々木から受け取る |
| Slack Bot Token | Slack への自動投稿 | ML Summary Bot | [Slack API](https://api.slack.com/apps) でアプリを作成して取得。権限がない場合は管理者から受け取る |
| Slack Incoming Webhook URL | Slack への自動投稿 | SprintSignal | [Slack API](https://api.slack.com/apps) で Incoming Webhooks を有効化して取得。権限がない場合は管理者から受け取る |
| Google OAuth | Google Slides / Drive へのアップロード | Slide Editor, PPTX Skill | 自分の Google アカウントで認証 (初回起動時のみ) |
| Gmail OAuth | メール取得 | ML Summary Bot | 自分の Google アカウントで認証 (初回起動時のみ) |

### 5. ツールをダウンロードして起動

使いたいツールの GitHub ページを開き、リポジトリ URL をコピーしてください。

Claude Code に以下を伝えてください。

> (リポジトリ URL) をクローンして、README に従ってセットアップしてください。Gemini API キーは (ここにキーを貼る) です。

Claude Code がクローン、依存パッケージのインストール、.env ファイルの作成、起動まで一括で行います。
