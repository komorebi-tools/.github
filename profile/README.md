# Komorebi Tools

コモレビ社内ツールのソースコードを管理・共同編集するための GitHub Organization です。

## リポジトリ一覧

| 分類 | リポジトリ | 概要 | 管理者 |
|---|---|---|---|
| Web ツール | [Report Studio](https://github.com/komorebi-tools/report-studio) | テキスト原稿から図解付き Web レポート / PDF を自動生成 | 安原 |
| Web ツール | [Slide Editor](https://github.com/komorebi-tools/slide-editor) | Markdown / テキストから PPTX スライドを自動生成 | 安原 |
| Claude Code スキル | [PPTX Skill](https://github.com/komorebi-tools/pptx-skill) | PowerPoint スライド生成エンジン | 安原 |
| Claude Code スキル | [FA Analysis Skill](https://github.com/komorebi-tools/fa-analysis-skill) | FA データ分析スキル | 安原 |
| 自動化 | [SprintSignal](https://github.com/komorebi-tools/sprint-signal) | スプシ「コンサル_プロジェクト管理」の更新を GAS で検知し、要約を Slack に自動投稿 | 佐々木 |
| 自動化 | [ML Summary Bot](https://github.com/komorebi-tools/ml-summary-bot) | consulting-team ML に届いたメールを要約し、Slack に自動投稿 | 佐々木 |
| 設計基盤 | [Design System](https://github.com/komorebi-tools/design-system) | コモレビのカラー、フォント、コンポーネント定義 + Claude Code スキル | 佐々木 |

## Claude Code でツール開発のはじめかた

### 1. ソフトウェアのインストール

各ツールを動かすために、PC に以下をインストールしておく必要があります。

| ソフトウェア | 使うツール |
|---|---|
| Node.js | Report Studio, Slide Editor, PPTX Skill |
| Python | Slide Editor, FA Analysis Skill, ML Summary Bot |

> SprintSignal は Google Apps Script で動くため、ブラウザだけあれば OK です。

Claude Code に以下のように伝えてください。

> Homebrew, Node.js, Python がインストールされているか確認して、入っていなければインストールしてください。

### 2. Claude Code の組織共通設定

全メンバーの Claude Code に共通のセキュリティ・品質ルールを適用します。初回のみ実行してください。

Claude Code に以下のように伝えてください。

> https://github.com/komorebi-tools/claude-code-org-setup をクローンして、setup.sh をターミナルで実行する手順を教えてください。

ターミナルで実行するよう案内されるので、そのとおりに実行してください。Mac のパスワードを聞かれるので入力します。

> 詳しいルール内容や FAQ は [Notion の「Claude Code 組織共通設定」ページ](https://www.notion.so/komorebi-inc/Claude-Code-33f13485e9d08044adc8d291c70149dc) を参照してください。

### 3. コモレビのデザインルールを適用する

UI や Web ページを作るとき、プロンプトに色やフォントを毎回書く必要はありません。

プロジェクトに設計ファイルを置いておくと、Claude Code がセッション開始時に自動で読み込んでくれます。

コモレビでは以下の 3 ファイルで品質を維持しています。

| ファイル | 役割 |
|---|---|
| DESIGN.md | 色、フォント、角丸、余白、シャドウを数値で定義。Claude Code はここを見てトークン準拠の CSS を書く |
| SKILL.md | デザイントークン、情報ソース、品質 3 層定義 (L1/L2/L3)、アンチパターン、チェックリスト。Claude Code の判断基準になる |
| rules.json | 絵文字禁止、#000000 禁止、全角括弧禁止など 9 件の禁止ルール。ファイル編集のたびに自動チェックし、違反があれば警告する |

この 3 つで「ルール定義 → 判断基準 → 自動検出」の流れができており、誰が作業しても品質がブレません。

プレビューページで全トークンをビジュアル確認できます → https://komorebi-tools.github.io/design-system/

コモレビの UI を作るプロジェクトには、Claude Code に以下を伝えてスキルを追加してください。

> komorebi-tools/design-system リポジトリから .claude/skills/komorebi-design-system/SKILL.md を取得して、このプロジェクトの同じパスに配置して。

詳しくは [design-system の README](https://github.com/komorebi-tools/design-system) を参照してください。

### 4. API キー、認証情報を用意する

| 種類 | 用途 | 使うツール | 取得方法 |
|---|---|---|---|
| Gemini API キー | 文章整理・図解生成・画像生成・メール要約など | Report Studio, Slide Editor, PPTX Skill, ML Summary Bot | 社内共通の課金済みキーを安原または佐々木から受け取る |
| Slack Bot Token | Slack への自動投稿 | ML Summary Bot | [Slack API](https://api.slack.com/apps) でアプリを作成して取得。権限がない場合は Slack ワークスペースの管理者から受け取る |
| Slack Incoming Webhook URL | Slack への自動投稿 | SprintSignal | [Slack API](https://api.slack.com/apps) で Incoming Webhooks を有効化して取得。権限がない場合は Slack ワークスペースの管理者から受け取る |
| Google OAuth | Google Slides / Drive へのアップロード | Slide Editor, PPTX Skill | 自分の Google アカウントで認証 (初回起動時のみ) |
| Gmail OAuth | メール取得 | ML Summary Bot | 自分の Google アカウントで認証 (初回起動時のみ) |

### 5. ツールをダウンロードして起動

使いたいツールの GitHub ページを開き、リポジトリ URL をコピーしてください。Claude Code に以下のように伝えてください。

> (リポジトリ URL) をクローンして、README に従ってセットアップしてください。Gemini API キーは (ここにキーを貼る) です。

Claude Code がクローン、依存パッケージのインストール、.env ファイルの作成、起動まで一括で行います。
