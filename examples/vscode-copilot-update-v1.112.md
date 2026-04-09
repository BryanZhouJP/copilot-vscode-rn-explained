# VS Code × GitHub Copilot アップデートレポート: v1.112

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.112
> **リリース日**: 2026-03-18
> **生成日**: 2026-04-10
> **対象読者**: クラウド・オンプレ基盤の企画・開発・運用を担当する IT 技術者（Windows + VS Code + GitHub Copilot 環境）

---

## 目次

- [新機能・変更一覧](#新機能変更一覧)
  - [AI活用提案サマリー](#ai活用提案サマリー)
- [習得ガイド（推奨レベル順 上位3件）](#習得ガイド)

---

## 新機能・変更一覧

> 10 件のアップデートを業務活用推奨レベル順に紹介します。

### AI活用提案サマリー

今回の VS Code × GitHub Copilot アップデートの中で、**クラウド・オンプレ基盤担当の IT 技術者**が特に注目すべき活用シナリオをまとめます。

| 推奨 | 機能名 | 活用シナリオ | 期待効果 |
| ---- | ------ | ----------- | -------- |
| ★★★ | Permissions levels in Copilot CLI | インフラ構成スクリプトや Terraform のリファクタリングを Autopilot で一気に自律実行 | 承認操作の削減・長時間タスクの自動化 |
| ★★★ | Customizations discovery in parent repositories | モノレポ配下のサービスごとリポジトリで、ルート共通の Copilot 手順書を自動参照 | チーム間のプロンプト品質統一 |
| ★★★ | Automatic symbol references | クラス・関数名をコピペするだけでエージェントが即座にコンテキスト取得 | 設計レビューや依存調査の高速化 |
| ★★☆ | /troubleshoot skill | カスタム指示が無視される・応答が遅い場合に自動診断 | トラブル原因の即特定 |
| ★☆☆ | Message steering in Copilot CLI | 長時間タスク中に途中修正や追加指示をキューとして送信 | タスク手戻りの削減 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. Permissions levels in Copilot CLI — Autopilot を含むエージェント権限レベル制御

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**Copilot CLI セッションに 3 段階の権限レベルが追加され、エージェントの自律度を細かく制御できるようになった。**

- `Default Permissions`：設定済みの承認フローに従い、ツール実行前に確認ダイアログを表示する
- `Bypass Approvals`：全ツール呼び出しを自動承認し、エラー発生時も自動リトライする（GA）
- `Autopilot`：質問応答まで含めて完全自律実行（`chat.autopilot.enabled` で有効化、Experimental）
- ローカルエージェントセッションと Copilot CLI セッションの両方に適用可能

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Permissions levels in Copilot CLI](https://code.visualstudio.com/updates/v1_112#_permissions-levels-in-copilot-cli)

💡 **AI 活用提案**:
- インフラ構成の大規模リファクタリング時に `Bypass Approvals` を使い、承認待ちによる中断なしにスクリプト修正を一気に進めることを検討してください
- 本番環境に影響がないステージング・テスト環境でのみ Autopilot を試験利用し、自律実行の安全性を先に確認してください

---

### 2. Customizations discovery in parent repositories — モノレポ向け設定ファイル自動探索

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**サブフォルダを開いた場合でも、Git リポジトリルートまで親ディレクトリを遡って Copilot カスタマイズファイルを自動検出できるようになった。**

- `chat.useCustomizationsInParentRepositories` 設定を有効にすることで機能する
- `copilot-instructions.md`・`AGENTS.md`・インストラクションファイル・プロンプトファイル・カスタムエージェント・スキル・フックすべてに適用
- 条件：開いているフォルダ自体が Git リポジトリでなく、親フォルダに `.git` が存在し、ワークスペース信頼済みであること

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Customizations discovery in parent repositories](https://code.visualstudio.com/updates/v1_112#_customizations-discovery-in-parent-repositories)

💡 **AI 活用提案**:
- モノレポ内の各マイクロサービスフォルダをサブワークスペースとして開くチームでは、リポジトリルートに共通の `copilot-instructions.md` を配置してコーディング規約やセキュリティガイドラインを一元管理することを検討してください

---

### 3. Automatic symbol references — コピー時にシンボル参照を自動挿入

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**クラス名・関数名・メソッド名などのシンボルをコピーしてチャット欄に貼り付けると、自動的に `#sym:Name` 形式のシンボル参照に変換され、エージェントがコンテキストを即座に取得できる。**

- テキストとして貼り付けたい場合は `Ctrl+Shift+V`（macOS: `Cmd+Shift+V`）で `Paste as Text` コマンドを使用する
- 別途 `#` で手動コンテキスト追加する手間が不要になり、質問の精度向上が期待できる
- 設定変更不要・インストール直後から動作する

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Automatic symbol references](https://code.visualstudio.com/updates/v1_112#_automatic-symbol-references)

💡 **AI 活用提案**:
- 大規模コードベースのコードレビュー時に、気になる関数名をコピペして「この関数の呼び出し元と副作用を教えて」と質問するだけで正確なコンテキストが渡せるようになります
- インフラ自動化スクリプトの依存関係調査にも活用できます

---

### 4. Enable or disable plugins and MCP servers — プラグイン・MCP サーバーの個別有効/無効切替

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | MCP |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**プラグインと MCP サーバーをアンインストールせずに有効/無効を切り替えられるようになった。グローバルとワークスペース単位でそれぞれ制御可能。**

- Extensions ビューまたは `Chat: Open Customizations` ビューから右クリックで操作
- プラグインの自動更新は `extensions.autoUpdate` 設定に従い動作（npm/pypi プラグインは承認が必要）
- プロジェクトごとに使用する MCP ツールをスコープ制限でき、意図しないツール呼び出しを防止できる

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Enable or disable plugins and MCP servers](https://code.visualstudio.com/updates/v1_112#_enable-or-disable-plugins-and-mcp-servers)

💡 **AI 活用提案**:
- 開発環境では DB アクセス系 MCP を有効にし、本番レビュー時には無効にするといった環境別ルールをワークスペース設定で管理することを検討してください

---

### 5. Debug web apps with the integrated browser — 統合ブラウザで Web アプリをデバッグ

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | その他 |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**VS Code に統合されたブラウザ上でブレークポイントのセットやステップ実行、変数の検査が VS Code を離れずに実行できるようになった。**

- 新しいデバッグタイプ `editor-browser` を `launch.json` に追加するか、既存の `chrome`/`msedge` 設定の `type` を変更するだけで移行可能
- 外部ブラウザを都度起動・切り替える手間がなくなり、フロントエンド系 IaC ダッシュボードの開発に効果的
- Windows 環境でも利用可能

📘 **用語解説**:
- **editor-browser**: VS Code 統合ブラウザ専用のデバッグタイプ。Chrome DevTools Protocol を内部で使用する

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Debug web apps with the integrated browser](https://code.visualstudio.com/updates/v1_112#_debug-web-apps-with-the-integrated-browser)
- [Integrated browser documentation](https://code.visualstudio.com/docs/debugtest/integrated-browser)

💡 **AI 活用提案**:
- 監視ダッシュボードや社内ポータルのフロントエンド開発時に、この機能を活用してブラウザ切り替えのオーバーヘッドを削減することを検討してください

---

### 6. `/troubleshoot` skill — エージェント動作の自動診断（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**チャットで `/troubleshoot` と入力することで、エージェントのデバッグログを自動解析し「なぜカスタム指示が適用されなかったか」「なぜ応答が遅かったか」を診断できる。**

- 事前に `github.copilot.chat.agentDebugLog.enabled` と `github.copilot.chat.agentDebugLog.fileLogging.enabled` を有効化して VS Code を再起動する必要がある
- JSONL 形式のデバッグログを解析し、ツール選択の理由・ネットワーク問題・遅延原因を可視化
- Preview 機能のため動作が変わる可能性がある

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Troubleshoot agent behavior with /troubleshoot](https://code.visualstudio.com/updates/v1_112#_troubleshoot-agent-behavior-with-troubleshoot-preview)

💡 **AI 活用提案**:
- カスタムエージェントや MCP 設定が期待通りに動かない場合の初動診断に取り入れることを検討してください
- デバッグログを有効化することで応答速度への若干の影響が出る場合があるため、通常業務と切り分けた検証環境で試すことを推奨します

---

### 7. Image and binary file support for agents — エージェントへの画像・バイナリファイル入出力（Experimental）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**エージェントがディスク上の画像ファイルとバイナリファイルをネイティブに読み込めるようになった。バイナリは hexdump 形式でエージェントに渡される。**

- エージェントが生成したスクリーンショットなどの画像は `chat.imageCarousel.enabled`（Experimental）で有効化した画像カルーセルに表示される
- Explorer ビューで画像ファイルや画像入りフォルダを右クリック → `Open Images in Carousel` で参照可能（`imageCarousel.explorerContextMenu.enabled` が必要）
- 現時点では Experimental 設定であり、本番利用前に評価が推奨される

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Image and binary file support for agents](https://code.visualstudio.com/updates/v1_112#_image-and-binary-file-support-for-agents)

💡 **AI 活用提案**:
- ネットワーク構成図（PNG）をチャットに添付して「この構成の冗長化上の問題点を指摘して」と依頼するユースケースを試してみてください

---

### 8. Sandbox locally running MCP servers — ローカル MCP サーバーのサンドボックス実行（macOS/Linux）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | MCP |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**ローカルで実行する stdio MCP サーバーをサンドボックス環境内で起動し、ファイルシステムとネットワークのアクセスを制限できるようになった。**

- `mcp.json` のサーバー定義に `"sandboxEnabled": true` を追加するだけで有効化
- 追加のフォルダやドメインへのアクセスが必要になった際は VS Code が権限付与を促すプロンプトを表示する
- **Windows では現時点で未対応**（WSL・SSH リモートは引き続き動作）

📘 **用語解説**:
- **stdio MCP サーバー**: ローカルプロセスとして起動し、標準入出力で通信する MCP サーバー形式

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Sandbox locally running MCP servers](https://code.visualstudio.com/updates/v1_112#_sandbox-locally-running-mcp-servers)

💡 **AI 活用提案**:
- macOS/Linux 開発機で社内システムにアクセスする MCP サーバーを運用している場合、サンドボックスで最小権限原則を実装することを検討してください
- Windows 環境での対応を待ちつつ、将来の展開に向けた評価を開始することを推奨します

---

### 9. Export and import agent debug logs — デバッグログのエクスポート・インポート（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**エージェントセッションのデバッグログをファイルにエクスポートし、他のメンバーが同じログをインポートして解析できるようになった。**

- `github.copilot.chat.agentDebugLog.enabled` が前提条件
- 50 MB を超えるファイルのインポート時は警告ダイアログが表示される（短いセッションのみ勧励）
- チーム横断的なトラブルシュートやサポート報告に活用できる
- Preview 機能であり挙動が変わる可能性がある

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Export and import agent debug logs](https://code.visualstudio.com/updates/v1_112#_export-and-import-agent-debug-logs-preview)

💡 **AI 活用提案**:
- チーム内でエージェント活用が進んだ段階で、問題事例のデバッグログを共有する仕組みの標準化を検討してください

---

### 10. Message steering and queueing in Copilot CLI — CLI セッションでのメッセージキューイング

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.112（2026-03-18） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**Copilot CLI セッションで、前のリクエスト処理中であっても追加メッセージを送信して応答方向を修正したり、後続タスクをキューに入れられるようになった。**

- ローカルエージェントセッションでは既に対応済みであり、今回 Copilot CLI セッションにも拡張
- 長時間タスクの中断なしに軌道修正が可能

🔗 **情報源**:
- [VS Code 1.112 Release Notes – Message steering and queueing in Copilot CLI](https://code.visualstudio.com/updates/v1_112#_message-steering-and-queueing-in-copilot-cli)

💡 **AI 活用提案**:
- Copilot CLI を活用した夜間 CI/CD パイプライン生成タスクで、エラー発生時に即座に修正指示を送り込む運用を将来的に検討してください

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: Permissions levels in Copilot CLI — Autopilot を含む 3 段階権限制御

#### 機能詳細解説

- エージェントに対して「どこまで自律的に動いてよいか」を明示的に設定できる仕組みで、過剰な承認中断と丸投げの中間を狙える
- `Default`：安全重視。ツール実行ごとに確認を求める既存動作
- `Bypass Approvals`：CI/CD 的な用途に最適。ツール呼び出しを自動承認し、エラー時も自動リトライして作業を続行する（正式リリース済み）
- `Autopilot`：完全自律。質問や確認なしにタスクを最後まで実行する。`chat.autopilot.enabled` が必要（Experimental）
- IT インフラ業務においては「ステージング環境の Terraform 計画実行 → レビュー → Apply」のような反復タスクで Bypass Approvals が特に有効
- 誤ったコマンドが自動実行されるリスクがあるため、本番リソースへのアクセス権を持った環境では慎重に使用する

#### 前提条件

- **VS Code バージョン**: v1.112 以上
- **GitHub Copilot プラン**: 個人 / Business / Enterprise すべて対応
- **必要な設定**: Copilot CLI を利用するには GitHub CLI (`gh`) と Copilot CLI 拡張が必要（`gh extension install github/gh-copilot`）
- **OS**: Windows 対応（Autopilot のみ Experimental）

#### セットアップ手順

1. **Autopilot を有効化する（オプション）**
   - コマンドパレット（`Ctrl+Shift+P`）→ `Preferences: Open Settings (JSON)` を開く
   - 以下を追加する：
     ```json
     "chat.autopilot.enabled": true
     ```

2. **Copilot CLI セッションを開始する**
   - チャットビューで `@cli` または `@copilot` とタイプしてセッションを開始する

3. **権限レベルを設定する**
   - チャットビューのセッションツールバーから権限レベルのドロップダウンを選択する
   - `Default Permissions` → `Bypass Approvals` → `Autopilot` の順で自律度が上がる

> 💡 **ヒント**: セッション開始後でも権限レベルの変更が可能です。タスクの内容に応じて途中で切り替えることもできます。

#### 使い方（ハンズオン）

**シナリオ**: Terraform の既存モジュール群を新しい命名規則に一括リファクタリングする大規模なタスクを Copilot CLI に任せる

**この機能がない場合**:
- ツールが呼び出されるたびに「許可しますか？」ダイアログが表示され、数十回の承認クリックが必要になる
- 長時間タスク（30 分超）でも人が画面の前に張り付く必要がある
- 承認待ちでエージェントがタイムアウトすることがある

**操作手順**:

1. VS Code でインフラリポジトリを開き、チャットを起動する
2. セッションツールバーで権限を **Bypass Approvals** に設定する
3. 「`/cli` terraform モジュール内の全 `snake_case` 変数を `camelCase` に変換して、テストが通ることを確認してください」と入力する
4. エージェントが自律的に変更・テスト実行・修正を繰り返す様子を監視する
5. 完了後にチャットの差分サマリーを確認し、問題があれば追加指示を送る

**期待される結果**:
- 承認ダイアログが表示されることなく、エージェントがツール呼び出しを連続実行する
- エラーが発生した場合も自動リトライされ、作業が完了するまで継続する
- セッション終了時に変更差分とテスト結果のサマリーが表示される

#### ベストプラクティス（IT インフラ業務向け）

- **段階的な自律度の引き上げ**: まず `Default` で動作確認 → `Bypass Approvals` での検証 → 安全性確認後に `Autopilot` を試すステップが安全
- **スコープの限定**: 変更対象ブランチを事前に切り離し、本番ブランチへの誤マージを防ぐ
- **ロールバック準備**: `git stash` または作業ブランチで作業し、想定外の変更に備える
- **Autopilot はレビュー必須の環境では使用しない**: 社内コンプライアンスで「全変更に人間のレビューが必要」な場合は `Bypass Approvals` を上限とする

#### 参考リンク

- [VS Code 1.112 Release Notes – Permissions levels in Copilot CLI](https://code.visualstudio.com/updates/v1_112#_permissions-levels-in-copilot-cli)
- [Copilot Agents – Autopilot and Agent Permissions](https://code.visualstudio.com/docs/copilot/agents/agent-tools#_permission-levels)

---

### 習得ガイド 2: Customizations discovery in parent repositories — モノレポ向け Copilot 設定共有

#### 機能詳細解説

- 大規模プロジェクトではモノレポ内の各パッケージやサービスをワークスペースとして開くことが多いが、これまでは各フォルダ内にしか Copilot 設定が効かなかった
- `chat.useCustomizationsInParentRepositories` を有効にすることで、Git リポジトリルートまでの全親フォルダの設定ファイルも参照するようになる
- 対象ファイルは `copilot-instructions.md`・`AGENTS.md`・`CLAUDE.md`・各種インストラクション・プロンプト・カスタムエージェント・スキル・フック
- チーム共通のコーディング規約・セキュリティルール・命名規則を一箇所で管理でき、メンバー全員が自動で適用できる状態になる
- 発見条件として「現在のワークスペースフォルダが Git リポジトリでない」「親フォルダに `.git` が存在する」「ワークスペース信頼が付与されている」のすべてが必要

#### 前提条件

- **VS Code バージョン**: v1.112 以上
- **GitHub Copilot プラン**: 個人 / Business / Enterprise
- **必要な設定**: `chat.useCustomizationsInParentRepositories` を有効化
- **OS**: Windows 対応

#### セットアップ手順

1. **設定を有効化する**
   - コマンドパレット（`Ctrl+Shift+P`）→ `Preferences: Open Settings (UI)` を開く
   - 検索欄に `chat.useCustomizationsInParentRepositories` と入力してチェックを入れる
   - または `settings.json` に以下を追加する：
     ```json
     "chat.useCustomizationsInParentRepositories": true
     ```

2. **リポジトリルートに共通インストラクションファイルを配置する**
   ```
   my-monorepo/
   ├── .github/
   │   └── copilot-instructions.md   ← 共通ルールを記述
   ├── packages/
   │   ├── service-a/                ← ここを VS Code で開いても上記が参照される
   │   └── service-b/
   ```

3. **サブフォルダを VS Code で開く**
   - `File > Open Folder` でモノレポ内のサービスフォルダを開く
   - Copilot チャットで `@workspace` に指示を出すと、ルートの共通設定が自動反映される

> 💡 **ヒント**: コマンドパレット → `Chat: Open Customizations` で現在有効になっている設定ファイルの一覧を確認できます。

#### 使い方（ハンズオン）

**シナリオ**: モノレポ内に複数の IaC サービスがある環境で、全サービス共通の Terraform 命名規則・Copilot 指示をルートに配置し、サービスフォルダを個別に開いても適用されることを確認する

**この機能がない場合**:
- 各サービスフォルダに同じ `copilot-instructions.md` をコピーして管理する必要がある
- 規約更新のたびに全フォルダを手動で更新する手間が生じる
- チームメンバーが古い指示を参照したまま作業するリスクがある

**操作手順**:

1. モノレポルートの `.github/copilot-instructions.md` に共通ルールを記述する（例：「Terraform リソース名はすべて `kebab-case` で記述すること」）
2. `chat.useCustomizationsInParentRepositories` を有効化する
3. VS Code で `packages/service-a` フォルダを開く
4. チャットに「新しい S3 バケットリソースを追加して」と入力する
5. 共通ルールがエージェントに反映され、命名規則に従ったコードが生成されることを確認する

**期待される結果**:
- ルートの `copilot-instructions.md` が有効になり、命名規則に沿ったリソース名が生成される
- `Chat: Open Customizations` 画面にルートのインストラクションファイルが表示される

#### ベストプラクティス（IT インフラ業務向け）

- **階層別の指示ファイル**: ルートにはチーム共通ルール、各サービスフォルダには固有のルールを配置して階層的に管理する
- **セキュリティガイドラインを共通化**: 「機密情報をコードにハードコードしない」「IAM ロールは最小権限で定義する」などを共通インストラクションに記述しておく
- **バージョン管理**: インストラクションファイルも Git で管理し、変更履歴を追跡できるようにする
- **ワークスペース信頼の確認**: 外部リポジトリをクローンした際に信頼が付与されているか確認する（セキュリティリスク回避）

#### 参考リンク

- [VS Code 1.112 Release Notes – Customizations discovery in parent repositories](https://code.visualstudio.com/updates/v1_112#_customizations-discovery-in-parent-repositories)
- [Agent Customizations overview](https://code.visualstudio.com/docs/copilot/customization/overview)

---

### 習得ガイド 3: Automatic symbol references — チャットへのシンボル参照の自動挿入

#### 機能詳細解説

- エディタ上でシンボル（クラス名・関数・メソッド・変数）をコピーしてチャット入力欄に貼り付けると、自動的に `#sym:SymbolName` 形式のコンテキスト参照に変換される
- エージェントは `#sym:` 参照を受け取ると、定義・型情報・使用箇所をコードベースから自動収集する
- 「このクラスの責務は？」「この関数の呼び出し元は？」のような質問の精度が劇的に向上する
- テキストとして貼り付けたい場合は `Ctrl+Shift+V`（Paste as Text）を使用する
- 設定変更不要・追加インストール不要

#### 前提条件

- **VS Code バージョン**: v1.112 以上
- **GitHub Copilot プラン**: 個人 / Business / Enterprise
- **必要な設定**: なし（デフォルト有効）
- **OS**: Windows 対応

#### セットアップ手順

1. **この機能は設定不要**
   - VS Code v1.112 以上にアップデートすれば即座に使用可能

2. **使い方の確認**
   - エディタ上のクラス名や関数名を選択して `Ctrl+C` でコピーする
   - チャット入力欄に `Ctrl+V` で貼り付けると `#sym:SymbolName` 形式に変換されることを確認する

> 💡 **ヒント**: テキストとして貼り付けたい場合は `Ctrl+Shift+V`（Paste as Text）を使用します。

#### 使い方（ハンズオン）

**シナリオ**: 大規模な IaC リポジトリで `TerraformModuleRegistry` クラスの依存関係と設計上のリスクを素早く把握したい

**この機能がない場合**:
- チャットで「TerraformModuleRegistry クラスを調査して」と入力しても、エージェントがクラス名を文字列として扱い、コンテキストを正確に収集できないことがある
- 手動で `#file:registry.tf` などを追加してコンテキストを補う手間が必要

**操作手順**:

1. エディタで `TerraformModuleRegistry` クラス名を選択し `Ctrl+C` でコピーする
2. チャット入力欄に `Ctrl+V` で貼り付けると `#sym:TerraformModuleRegistry` に変換されることを確認する
3. 「`#sym:TerraformModuleRegistry` は何に依存していますか？循環依存のリスクはありますか？」と入力して送信する
4. エージェントが定義・インポート・呼び出し箇所を自動収集して分析結果を返す

**期待される結果**:
- エージェントが `TerraformModuleRegistry` の定義・メソッド・依存クラスを自動的に解析する
- ファイルパスやメソッド名が正確な参照として回答に含まれる

#### ベストプラクティス（IT インフラ業務向け）

- コードレビュー前に修正対象のクラスや関数を貼り付けて「このロジックに潜在的なバグや改善点はあるか」と質問する
- 複数シンボルを一度に貼り付けることも可能。関連する複数の関数を貼り付けて「一緒に見たときの設計上の問題を教えて」と使うと効果的

#### 参考リンク

- [VS Code 1.112 Release Notes – Automatic symbol references](https://code.visualstudio.com/updates/v1_112#_automatic-symbol-references)
- [Copilot Chat – Chat context](https://code.visualstudio.com/docs/copilot/chat/copilot-chat#_chat-context)
