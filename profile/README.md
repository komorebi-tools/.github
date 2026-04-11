# Komorebi Tools

コモレビ社内ツールのソースコードを管理&#xB7;共同編集するための GitHub Organization です。

1リポジトリ = 1ツールで構成されています。

## ツール&#xB7;スキル一覧

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

### デザインルール (UI を作るプロジェクトに都度追加)

[design-system](https://github.com/komorebi-tools/design-system) で管理しています。[ビジュアルプレビュー](https://komorebi-tools.github.io/design-system/)もあります。

| ファイル | 役割 |
|---|---|
| [DESIGN.md](https://github.com/komorebi-tools/design-system/blob/main/DESIGN.md) | 色、フォント、角丸、余白、シャドウを数値で定義。Claude Code はここを見てトークン準拠の CSS を書く |
| [SKILL.md](https://github.com/komorebi-tools/design-system/blob/main/.claude/skills/komorebi-design-system/SKILL.md) | デザイントークン、情報ソース、品質 3 層定義 (L1/L2/L3)、アンチパターン、チェックリスト。Claude Code の判断基準になる |
| [contracts/rules.json](https://github.com/komorebi-tools/design-system/blob/main/contracts/rules.json) | 絵文字禁止、#000000 禁止、全角括弧禁止など 9 件の禁止ルール。ファイル編集のたびに自動チェックし、違反があれば警告する |

### 作業ルール (初回セットアップで自動で入る)

各メンバーの PC に設定されます。[詳細は Notion](https://www.notion.so/komorebi-inc/Claude-Code-33f13485e9d08044adc8d291c70149dc) を参照してください。**自分用のルールを追加したい場合は、CLAUDE.md や settings.json を自由に編集して OK です。** 組織共通の内容を消さないようにだけ注意してください。

| ファイル | 役割 |
|---|---|
| [CLAUDE.md](https://github.com/komorebi-tools/claude-code-org-setup/blob/main/managed-CLAUDE.md) | 「読んでいないコードは変更するな」「絵文字禁止」「Markdown の改行ルール」など、全プロジェクト共通の作業ルール |
| ~/.claude/settings.json | 権限設定、deny ルール (rm -rf / force push のブロック)、hook 設定、effortLevel など |
| ~/.claude/hooks/design-check.sh | Edit / Write のたびに rules.json の禁止パターンを自動チェックする hook |

settings.json と hooks は各メンバーの PC にのみ存在し、GitHub 上にはテンプレートがありません。

### これらがどう連携するか

1. **CLAUDE.md** が全プロジェクト共通の作業ルールを定義する
2. **DESIGN.md / SKILL.md** がデザインの数値基準と判断基準を与える
3. **settings.json の deny ルール** が破壊的コマンド (rm -rf / force push) をブロックする
4. **rules.json + hook** がファイル編集のたびにデザイン違反を自動検出する

各ファイルの詳細は [design-system の README](https://github.com/komorebi-tools/design-system) を参照してください。

導入手順は下の「はじめかた」を参照してください。

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

全メンバー共通のセキュリティ&#xB7;品質ルールを適用します。初回のみ実行してください。

Claude Code に以下を伝えてください。

> komorebi-tools/claude-code-org-setup をクローンして、README の手順に従ってセットアップしてください。

詳しいルール内容や FAQ は [Notion の「Claude Code 組織共通設定」ページ](https://www.notion.so/komorebi-inc/Claude-Code-33f13485e9d08044adc8d291c70149dc) を参照してください。

### 3. デザインスキルを追加する

コモレビの UI を作るプロジェクトには、デザインスキルを追加してください。

Claude Code に以下を伝えてください。

> komorebi-tools/design-system リポジトリから .claude/skills/komorebi-design-system/SKILL.md を取得して、このプロジェクトの同じパスに配置して。

これで「コモレビの LP を作って」と伝えるだけで、デザインルールに沿った UI が生成されます。

### 4. 開発したいツールの API キー、認証情報を用意する

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

---

## 開発ルール (仮)

現在は少人数チームのため、スピード重視のシンプルなルールにしています。チーム規模が大きくなったら見直します。

- 作業を始める前に、Claude Code に「最新の状態を pull して」と伝えてください。他のメンバーの変更を取り込んでから作業することで、コンフリクト (変更の衝突) を防げます。
- **コードを変更したらそのまま push して OK です。** 特別な手順 (ブランチ作成や承認依頼) は不要です。
- 変更を GitHub に反映するときは、Claude Code に「コミットして push して」と伝えてください。他のメンバーも最新の状態を使えるようになります。
- push した後にツールが動かなくなった場合は、Claude Code に「さっきの変更を元に戻して」と伝えてください。Git の履歴から前の状態に戻してくれます。
- .env やパスワードなど機密情報を含むファイルは push しないでください。各ツールの .gitignore で .env は除外されているはずですが、不安なら Claude Code に「.gitignore に .env が入っているか確認して」と聞いてください。
- 他の人が使っているツールを大きく変えるときは、同時に作業してコンフリクトするのを避けるため、Slack で一声かけてから作業してください。
