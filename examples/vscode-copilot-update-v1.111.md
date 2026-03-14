# VS Code × GitHub Copilot アップデートレポート: v1.111

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.111
> **リリース日**: 2026年3月9日
> **生成日**: 2026年3月14日
> **対象読者**: クラウド・オンプレ基盤の企画・開発・運用を担当する IT 技術者（Windows + VS Code + GitHub Copilot 環境）

---

## 目次

- [新機能・変更一覧](#新機能変更一覧)
  - [AI活用提案サマリー](#ai活用提案サマリー)
- [習得ガイド（推奨レベル順 上位3件）](#習得ガイド)

---

## 新機能・変更一覧

> 9 件のアップデートを業務活用推奨レベル順に紹介します。

### AI活用提案サマリー

今回の VS Code × GitHub Copilot アップデートの中で、**クラウド・オンプレ基盤担当の IT 技術者**が特に注目すべき活用シナリオをまとめます。

| 推奨 | 機能名 | 活用シナリオ | 期待効果 |
| ---- | ------ | ----------- | -------- |
| ★★★ | Autopilot ＆ エージェント権限ピッカー | CI/CDスクリプト生成・インフラコードのリファクタリングをエージェントに委任し、人手の介入なしに完了させる | 従来手動承認が必要だったエージェント作業を全自動化し、長時間タスクの工数を大幅削減 |
| ★★★ | デバッグイベントスナップショット | カスタムエージェントが期待通り動作しない際に、ログスナップショットをCopilotに渡してトラブルシューティングを即実施 | エージェント不具合の原因特定時間を短縮し、カスタマイズ品質を向上 |
| ★★★ | GPT-5.4 モデル GA | エージェントモードでのインフラコード大規模リファクタリングや複数ファイル横断の問題調査に高精度モデルを活用 | マルチステップ・複雑な推論が必要な作業の成功率向上 |
| ★★☆ | エージェントスコープ付きフック | インフラ自動化エージェント専用のフックを定義し、ファイル変更後の検証スクリプトを自動実行 | エージェント作業の品質保証を自動化し、誤操作による障害リスクを低減 |
| ★★☆ | Copilot コードレビューのエージェント化 | PRマージ前にCopilotがエージェントとして自律的にセキュリティ・品質レビューを実施 | コードレビュープロセスの自動化により、リリースサイクルを短縮 |
| ★★☆ | Copilot Memory デフォルト有効化 | Pro/Pro+ユーザーがCopilotとの会話履歴・コンテキストをまたいで活用できるようになる | プロジェクト固有情報の再入力不要で、反復作業の効率化 |
| ★☆☆ | チャットヒントの改善 | 新しいチャット機能（/init、/fork 等）を体系的に習得するためのオンボーディング活用 | チーム全員がCopilotの最新機能を効率的に習得できる |
| ★☆☆ | AI CLIプロファイルグループ | ターミナルからGitHub Copilot CLIへのアクセスが迅速化し、CLIベースの自動化作業が効率化 | Copilot CLI活用の導線を整備し、ターミナル操作の生産性向上 |
| ★☆☆ | Edit Mode廃止予定（v1.125で完全削除） | Edit Modeを使用している場合は今のうちに代替機能（Copilot Edits等）への移行を計画 | v1.125以降の機能停止リスクを事前に回避 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. Autopilot ＆ エージェント権限ピッカー

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.111（2026年3月9日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**Agent Modeの自律度をセッション単位で切り替えられる権限ピッカーが登場し、承認不要で完全自律動作する Autopilot が利用可能に。**

- チャットビューの入力欄に権限ピッカーが追加され、`Default Approvals`（手動承認）/ `Bypass Approvals`（全ツール自動承認・エラー時自動リトライ）/ `Autopilot（Preview）`（全自動＋質問への自動応答）の3段階から選択できる
- Autopilot はエージェントが `task_complete` ツールを呼び出すまで自律的に繰り返し作業を継続する仕組みで、長時間タスクの完全委任が可能
- Stable版では `chat.autopilot.enabled` 設定で有効化が必要（Insiders版はデフォルト有効）。権限レベルは現在のセッションにのみ適用され、セッション内でいつでも変更可能
- **注意**: ファイル編集・ターミナルコマンド・外部ツール呼び出し等の破壊的操作も無確認で実行されるため、本番環境への誤適用リスクに注意

📘 **用語解説**:
- **Autopilot**: エージェントがタスク完了まで完全自律で動作するモード。人間の介入なしに反復・修正・実行を繰り返す
- **task_complete ツール**: エージェントがタスク完了を宣言するための内部ツール。Autopilot を終了させるシグナルとなる

🔗 **情報源**:
- [Visual Studio Code 1.111 Release Notes](https://code.visualstudio.com/updates/v1_111)
- [Use tools with agents - Permission levels](https://code.visualstudio.com/docs/copilot/agents/agent-tools#permission-levels)

💡 **AI 活用提案**:
- インフラコードのリファクタリングタスクを Autopilot に委任し、エージェントが複数ファイルにまたがる修正を自律的に完了させる運用を検討する
- CI/CDパイプラインのスクリプト生成など、試行錯誤が多いタスクで Bypass Approvals を活用し、承認ダイアログなしにスムーズな反復作業を実現する

---

### 2. デバッグイベントスナップショット（`#debugEventsSnapshot`）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.111（2026年3月9日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode / Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**エージェントのデバッグログスナップショットをチャットコンテキストとして添付し、Copilot 自身に動作トラブルを分析・診断させられるようになった。**

- `#debugEventsSnapshot` をチャット入力で参照すると、現在のエージェントセッションのデバッグイベント一覧をコンテキストとして添付できる
- 「どのカスタマイズが読み込まれたか」「トークン消費量はいくらか」「ツール呼び出しは何回発生したか」などをCopilotに質問できる
- Agent Debugパネル右上のスパークルアイコン（✨）を選択しても同じ操作が可能。添付ファイルをクリックするとスナップショット取得時刻でフィルタされたログが表示される
- カスタムエージェントやMCPサーバーの動作確認・トラブルシューティングに直接活用可能

📘 **用語解説**:
- **Agent Debug パネル**: エージェントセッション中のイベントをツール呼び出し・LLMリクエスト・サブエージェント連携等を時系列に可視化するパネル

🔗 **情報源**:
- [Visual Studio Code 1.111 Release Notes](https://code.visualstudio.com/updates/v1_111)
- [Debug chat interactions](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view)

💡 **AI 活用提案**:
- MCPサーバーが期待通り呼び出されない場合、`#debugEventsSnapshot` をチャットに添付して「このログを見てMCPツールが呼ばれていない原因を教えて」と質問することで、迅速な原因特定が可能
- カスタムエージェントのトークン消費が多い場合、スナップショットを元にプロンプト最適化の提案をCopilotに求めることができる

---

### 3. GPT-5.4 が GitHub Copilot で利用可能に（GA）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | GitHub Changelog（2026年3月5日） |
| カテゴリ | 新機能（モデル追加） |
| 対象コンポーネント | コード補完 / Copilot Chat / Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**OpenAI の最新エージェント特化モデル GPT-5.4 が GitHub Copilot で正式 GA となり、VS Code のモデルピッカーから選択可能に。**

- GPT-5.4 はエージェント・コーディング・多段推論に特化したモデルで、複雑なマルチステップタスクの成功率が向上
- VS Code v1.104.1 以降のすべてのモード（chat / ask / edit / agent）で選択可能
- Pro / Pro+ / Business / Enterprise ユーザーが利用対象。**Business・Enterprise は管理者がポリシー設定で GPT-5.4 を有効化する必要あり**
- Agent Modeでのインフラコード大規模変更や複数ファイル横断の分析タスクで特に性能向上が期待できる

📘 **用語解説**:
- **モデルピッカー**: Copilot Chat 入力欄でモデルを切り替えるUI。用途に応じて高性能モデルと軽量モデルを使い分けられる

🔗 **情報源**:
- [GPT-5.4 is generally available in GitHub Copilot](https://github.blog/changelog/2026-03-05-gpt-5-4-is-generally-available-in-github-copilot)
- [GitHub Copilot supported models](https://docs.github.com/copilot/reference/ai-models/supported-models)

💡 **AI 活用提案**:
- インフラ構成レビューや大規模なTerraform/Ansibleコードの分析・リファクタリングに GPT-5.4 を指定し、精度向上の効果を評価することを推奨
- Business・Enterprise の管理者はポリシー設定を確認し、チームが最新モデルを利用できるようアクセスを有効化することを検討する

---

### 4. エージェントスコープ付きフック（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.111（2026年3月9日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**カスタムエージェントの `.agent.md` フロントマターに `hooks` セクションを定義することで、そのエージェントが有効なときだけ前処理・後処理ロジックを実行できるようになった。**

- 有効化には `chat.useCustomAgentHooks` 設定を `true` にする必要がある（Preview機能）
- ワークスペース全体のフックと異なり、特定エージェント専用のフックとして動作するため、他のチャット操作に影響しない
- ファイル編集後の自動フォーマット・テスト実行、セッション開始時のプロジェクト状態検証、危険なコマンドのブロックなどに活用可能
- フックは JSON形式で`PreToolUse` / `PostToolUse` / `SessionStart` / `Stop` 等8種類のライフサイクルイベントをサポート

🔗 **情報源**:
- [Visual Studio Code 1.111 Release Notes](https://code.visualstudio.com/updates/v1_111)
- [Agent hooks - Agent-scoped hooks](https://code.visualstudio.com/docs/copilot/customization/hooks#_agentscoped-hooks)

💡 **AI 活用提案**:
- インフラ構成変更専用エージェントを作成し、ファイル変更後に構文チェック（例: `terraform validate`）を自動実行するフックを定義することで、エージェントが生成した設定の品質を自動保証できる
- セキュリティポリシー違反コマンド（`rm -rf`、`DROP TABLE`等）を PreToolUse フックでブロックする仕組みを構築し、エージェントの暴走リスクを低減する

---

### 5. Copilot コードレビューがエージェントアーキテクチャに移行

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | GitHub Changelog（2026年3月5日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**GitHub Copilot コードレビュー機能がエージェントアーキテクチャに刷新され、より高精度かつ文脈を理解した自律的なコードレビューが可能になった。**

- コードレビュー機能が単発の LLM 呼び出しから、エージェントが反復的に検証・分析を行うアーキテクチャへ移行
- セキュリティ問題・パフォーマンス問題・コーディング規約違反などの検出精度向上が期待できる
- GitHub.com 上の PR に対して `@copilot review` を実行することで利用可能
- VS Code 内での Copilot コードレビューにも今後この改善が反映される予定

🔗 **情報源**:
- [Copilot code review now runs on an agentic architecture](https://github.blog/changelog/2026-03-05-copilot-code-review-now-runs-on-an-agentic-architecture)

💡 **AI 活用提案**:
- インフラコード（Terraform、Dockerfile、Kubernetes マニフェスト等）の PR に対して Copilot コードレビューを標準化し、セキュリティ設定ミスやベストプラクティス違反の自動検出体制を構築することを検討する

---

### 6. Copilot Memory が Pro/Pro+ ユーザーでデフォルト有効化

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | GitHub Changelog（2026年3月4日） |
| カテゴリ | 動作変更 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**GitHub Copilot Pro / Pro+ ユーザーでメモリ機能がデフォルト ON に変更され、会話をまたいでコンテキストを保持するようになった（パブリックプレビュー）。**

- Copilot がプロジェクト設定・ユーザーの好み・過去の会話内容を記憶し、次回以降のチャットに反映できる
- 業務でよく使うプロジェクト固有のコンテキスト（命名規則・アーキテクチャ方針等）を毎回説明する手間が減少
- **Business・Enterprise プランは対象外**（現時点ではパブリックプレビューとして Pro/Pro+ のみ）
- プライバシーを重視する場合は設定でオフにすることもできる

🔗 **情報源**:
- [Copilot Memory now on by default for Pro and Pro+ users in public preview](https://github.blog/changelog/2026-03-04-copilot-memory-now-on-by-default-for-pro-and-pro-users-in-public-preview)

💡 **AI 活用提案**:
- Pro/Pro+ ユーザーは Copilot Memory を活用して「このプロジェクトの命名規則はスネークケース」「Terraformでは常にモジュール化する」などのルールを記憶させ、毎回説明なしで一貫したコード提案を受けることを試してみる

---

### 7. チャットヒントの改善

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.111（2026年3月9日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**チャットヒントがオンボーディングジャーニー形式に再設計され、機能発見性が向上した。**

- 基礎的なヒント（Planエージェントの使い方・カスタムエージェント作成）が優先表示され、次いでクオリティ向上ヒントがランダム表示される
- `/init`（プロジェクト設定初期化）と `/fork`（会話の分岐）のスラッシュコマンドに対応したヒントが新たに追加
- ヒントにキーボードショートカットが表示されるようになり、機能習得が促進される
- 複数 Chat エディタを開いている場合はヒントが非表示になりクラッタを軽減

🔗 **情報源**:
- [Visual Studio Code 1.111 Release Notes](https://code.visualstudio.com/updates/v1_111)

💡 **AI 活用提案**:
- Copilot を使い始めたチームメンバーへのオンボーディングで、チャットヒントを活用した自己学習フローを取り入れることを検討する

---

### 8. AI CLI プロファイルグループ in ターミナルドロップダウン（Experimental）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.111（2026年3月9日） |
| カテゴリ | 新機能（実験的） |
| 対象コンポーネント | その他 |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**GitHub Copilot CLI などの AI CLI ターミナルプロファイルがターミナルドロップダウン上部に専用グループとして表示されるようになった。**

- 有効化には `terminal.integrated.experimental.aiProfileGrouping` を `true` に設定する
- GitHub Copilot CLI はターミナルでのコマンド提案・説明・変換機能を持ち、インフラ担当者が CLI コマンドを素早く生成するのに役立つ
- 実験的機能のため動作変更の可能性あり

🔗 **情報源**:
- [Visual Studio Code 1.111 Release Notes](https://code.visualstudio.com/updates/v1_111)

💡 **AI 活用提案**:
- GitHub Copilot CLI を活用している場合、この設定でアクセス性を改善し、ターミナル操作の起点として AI CLI を積極活用する環境を整備することを検討する

---

### 9. Edit Mode 廃止予定（v1.125 で完全削除）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.111（2026年3月9日）※ v1.110で廃止予告済み |
| カテゴリ | 廃止予定 |
| 対象コンポーネント | Copilot Edits |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**Edit Mode が v1.110 で公式廃止予定となり、v1.125 で完全削除される。移行猶予期間中は `chat.editMode.hidden` 設定で一時的な再有効化が可能。**

- v1.125（2026年後半予定）以降は `chat.editMode.hidden` 設定でも再有効化できなくなる
- Edit Mode の代替は Copilot Edits（`Ctrl+Shift+I`）または Agent Mode
- 現在 Edit Mode を使っているチームは代替機能への移行計画を早めに策定することを推奨

🔗 **情報源**:
- [Visual Studio Code 1.111 Release Notes - Deprecated features](https://code.visualstudio.com/updates/v1_111)

💡 **AI 活用提案**:
- Edit Mode を使ったワークフローを Copilot Edits または Agent Mode に移行するための検証を早期に開始し、チーム内の手順書・ドキュメントを更新することを推奨する

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: Autopilot ＆ エージェント権限ピッカー

#### 機能詳細解説

- v1.111 でチャットビューの入力エリアに「権限ピッカー」が統合され、エージェントの自律度を `Default Approvals` / `Bypass Approvals` / `Autopilot` の3段階でコントロールできるようになった
- **Default Approvals**: 従来通り。ツール呼び出しのたびに確認ダイアログが表示される
- **Bypass Approvals**: すべてのツール呼び出しを自動承認。エラー発生時に自動リトライ。ファイル編集・ターミナルコマンド実行・外部ツール呼び出しも無確認で実行される
- **Autopilot（Preview）**: Bypass Approvals に加え、エージェントが質問・入力待ちになった場合も自動応答し、`task_complete` ツールを呼び出すまで完全自律で動作し続ける
- IT インフラ担当者にとって、Autopilot はコード生成・ファイル作成・コマンド実行を含む複雑な自動化タスク（例: Terraform コードの一括リファクタリング）を委任する際に特に有効
- セキュリティリスク: 破壊的操作（ファイル削除・本番環境へのコマンド実行等）も自動承認されるため、利用前に作業スコープを明確に指示することが重要

#### 前提条件

- **VS Code バージョン**: v1.111 以上
- **GitHub Copilot プラン**: Individual / Business / Enterprise（すべてのプランで利用可）
- **必要な設定**: Autopilot は Stable 版では `chat.autopilot.enabled` を `true` に設定する（Insiders 版はデフォルト有効）
- **OS**: Windows（本ガイドは Windows 環境での手順を前提とする）

#### セットアップ手順

1. **Autopilot の有効化（Stable 版のみ）**
   ```json
   // settings.json に追加
   "chat.autopilot.enabled": true
   ```
   または、コマンドパレット（`Ctrl+Shift+P`）→ `Preferences: Open Settings (JSON)` で settings.json を開いて追記する

2. **権限レベルの変更**
   Chat ビュー（`Ctrl+Alt+I`）を開き、入力欄の左下にある権限ピッカードロップダウンから希望のレベルを選択する

3. **初回有効化の確認**
   Bypass Approvals または Autopilot を初めて選択すると、セキュリティ警告ダイアログが表示される。リスクを理解した上で確認ボタンを押す

> 💡 **ヒント**:
> - コマンドパレット: `Ctrl+Shift+P` → `Chat: Open Chat` でチャットビューを開ける
> - 権限レベルはセッション中にいつでも変更可能。リスクが高い作業前は Default Approvals に戻すことを推奨

#### 使い方（ハンズオン）

**シナリオ**: 複数のTerraformファイルにまたがるリソース命名規則の一括修正をエージェントに委任する

**この機能がない場合**:
- エージェントがファイルを修正するたびに確認ダイアログで「Allow」を押す必要がある
- 30ファイル修正する場合、30回の手動承認が必要で中断が続き、作業集中が妨げられる
- エージェントがツールを呼ぶたびに待機するため、全体の処理時間が延びる

**操作手順**:

1. Chat ビューを開き（`Ctrl+Alt+I`）、権限ピッカーから `Autopilot` を選択する
2. 以下のようにプロンプトを入力する：
   ```
   このワークスペース内のすべての Terraform ファイルを確認し、
   リソース名の命名規則を snake_case から kebab-case に一括変換してください。
   変更完了後、変更したファイルの一覧を教えてください。
   ```
3. エージェントが自律的にファイルの読み込み・変更・検証を繰り返し、完了を宣言するまで待つ

**期待される結果**:
- エージェントが確認ダイアログなしにすべてのファイルを自律的に修正する
- 途中でエラーが発生した場合も自動リトライし、問題を解決するまで作業を継続する
- 完了時に変更ファイル一覧と変更内容のサマリーが返される

#### ベストプラクティス（IT インフラ業務向け）

- Autopilot を使用する前に、エージェントに対して「本番環境のファイルには触れないこと」「変更前にバックアップを作成すること」などの制約を明示的にプロンプトに含める
- 長時間タスクを委任する場合は、作業途中でいつでも停止ボタンを押せる状態で待機することを推奨（完全な自動化が成熟するまで）
- 本番適用前にステージング環境や Git ブランチを分離した状態でテストし、結果を確認してから本番へマージする運用を徹底する
- セキュリティポリシー上問題がある環境では、代わりに `chat.tools.terminal.sandbox` 設定でターミナルコマンドのサンドボックス化を検討する

#### 参考リンク

- [Visual Studio Code 1.111 Release Notes](https://code.visualstudio.com/updates/v1_111)
- [Use tools with agents - Permission levels](https://code.visualstudio.com/docs/copilot/agents/agent-tools#permission-levels)
- [Security considerations for using AI in VS Code](https://code.visualstudio.com/docs/copilot/security)

---

### 習得ガイド 2: デバッグイベントスナップショット（`#debugEventsSnapshot`）

#### 機能詳細解説

- Agent Debug パネルに記録されているエージェントのイベントログをスナップショットとしてチャットコンテキストに添付できる
- Copilot 自身を使ってエージェントの動作問題（カスタマイズが読み込まれない・MCPツールが呼ばれない・トークンが多すぎる等）を分析・解決するための自己診断機能
- スナップショットには「セッション内に発生したイベント（ツール呼び出し・LLMリクエスト・サブエージェント連携等）」が含まれる
- 添付ファイルをクリックすると、スナップショット取得時刻でフィルタされた Agent Debug パネルのログが表示されるため、詳細確認も容易
- Agent Debug パネルは以下3つのビューを持つ: Logs（時系列イベント）、Summary（統計情報）、Agent Flow Chart（フロー可視化）

#### 前提条件

- **VS Code バージョン**: v1.111 以上
- **GitHub Copilot プラン**: すべてのプラン
- **必要な設定**: 特になし（デフォルト有効）
- **OS**: Windows（本ガイドは Windows 環境での手順を前提とする）

#### セットアップ手順

1. **Agent Debug パネルを開く**
   - Chat ビュー右上のギアアイコン（設定）→「Show Agent Logs」を選択
   - または `Ctrl+Shift+P` → `Developer: Open Agent Debug Panel` を実行

2. **デバッグイベントスナップショットを添付する方法（方法A: テキスト参照）**
   Chat ビューの入力欄に以下を入力する：
   ```
   #debugEventsSnapshot
   なぜMCPのfetchツールが呼ばれないのか教えてください
   ```

3. **デバッグイベントスナップショットを添付する方法（方法B: Agent Debug パネルから）**
   Agent Debug パネルの右上にある✨（スパークル）アイコンをクリックすると、チャット入力欄にスナップショットが添付された状態で開く

> 💡 **ヒント**:
> - `Ctrl+Shift+P` → `Developer: Open Agent Debug Panel` でパネルを直接開ける
> - Summary ビューはパネル上部のブレッドクラムでセッション名を選択すると開く

#### 使い方（ハンズオン）

**シナリオ**: カスタムエージェントに MCP サーバーのツールを使わせようとしているが、エージェントがツールを無視してしまう問題を調査したい

**この機能がない場合**:
- MCP サーバーのログを別ウィンドウで確認し、VS Code のログと照合する作業が必要
- どのタイミングでツールが呼ばれたか・呼ばれなかったかの特定に時間がかかる
- Copilot に問題を説明するためにログを手動でコピー＆ペーストする手間がかかる

**操作手順**:

1. Agent Debug パネルを開き（`Ctrl+Shift+P` → `Developer: Open Agent Debug Panel`）、問題が発生したチャットセッションのイベントを確認する
2. パネル右上の✨アイコンをクリックし、スナップショットをチャットに添付する
3. 以下のように Copilot に質問する：
   ```
   添付のデバッグスナップショットを確認して、
   MCPサーバーの「fetch」ツールが呼び出されていない原因を分析してください。
   ツール一覧に表示されているかも確認してください。
   ```

**期待される結果**:
- Copilot がスナップショットを分析し、「ツールが System prompt に含まれていない」「MCP サーバーへの接続が確立されていない」などの具体的な原因を特定する
- 問題の解決手順（設定の修正方法・再起動手順等）を提示する

#### ベストプラクティス（IT インフラ業務向け）

- カスタムエージェントや MCP サーバーを新規導入した際は、最初のテスト段階でスナップショットを取得して Copilot に動作確認させる習慣をつけると効率的
- トークン消費量が多いと感じたら Summary ビューで統計確認し、スナップショットを添付して「トークン消費を削減するためのプロンプト最適化提案をしてください」と依頼する

#### 参考リンク

- [Visual Studio Code 1.111 Release Notes](https://code.visualstudio.com/updates/v1_111)
- [Debug chat interactions](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view)

---

### 習得ガイド 3: GPT-5.4 モデル GA

#### 機能詳細解説

- GPT-5.4 は OpenAI のエージェント専用・コーディング特化モデルで、実世界のソフトウェア開発タスクにおいて従来モデルを上回る成功率を達成
- 複数ファイル横断の変更・複雑なリファクタリング・マルチステップのデバッグ等で特にパフォーマンス向上が期待できる
- VS Code v1.104.1 以降の全モード（chat / ask / edit / agent）のモデルピッカーから選択可能
- **Business/Enterprise プランは管理者がポリシーで有効化する必要がある点に注意**

#### 前提条件

- **VS Code バージョン**: v1.104.1 以上（推奨: 最新版 v1.111）
- **GitHub Copilot プラン**: Pro / Pro+ / Business / Enterprise
- **必要な設定**: Business/Enterprise は org 管理者が GitHub 設定の Copilot ポリシーで GPT-5.4 を有効化する必要あり
- **OS**: Windows（本ガイドは Windows 環境での手順を前提とする）

#### セットアップ手順

1. **モデルピッカーで GPT-5.4 を選択する**
   Chat ビュー（`Ctrl+Alt+I`）入力欄のモデルピッカードロップダウンから「GPT-5.4」を選択する

2. **Business/Enterprise 管理者向け: ポリシー有効化**
   GitHub.com の Organization 設定 → Copilot → Policies → 「GPT-5.4」をチェックして保存する

> 💡 **ヒント**:
> - モデルピッカーはチャット入力欄の左横にあるドロップダウンメニュー
> - セットアップ後、モデルはセッションをまたいで記憶される

#### 使い方（ハンズオン）

**シナリオ**: 大規模な Terraform コードベースのセキュリティ監査と改善提案を GPT-5.4 に依頼する

**この機能がない場合**:
- 従来モデルでは複雑なリソース依存関係を正確に把握しきれず、見落としが発生する可能性がある
- セキュリティ問題の原因と修正方法の説明が不正確または不完全なことがある

**操作手順**:

1. Chat ビューでモデルピッカーから「GPT-5.4」を選択する
2. Agent Mode に切り替えて以下のプロンプトを入力する：
   ```
   #codebase このプロジェクトのTerraformコードにセキュリティ上の問題がないか
   調査してください。具体的には以下を確認してください：
   - IAMポリシーの最小権限原則の遵守
   - セキュリティグループの過剰なポート開放
   - 暗号化設定の漏れ
   問題があれば具体的な修正方法も提示してください。
   ```

**期待される結果**:
- コードベース全体を横断して問題を特定し、ファイル名・行番号とともに具体的な問題点を列挙する
- 修正コードの例示を含む、実装可能な改善提案を提供する

#### ベストプラクティス（IT インフラ業務向け）

- GPT-5.4 はトークン消費が多い場合があるため、軽いタスク（コード説明・単純な変換）には軽量モデルを使い分けることで効率化する
- Business/Enterprise 管理者は新モデル導入前に組織のセキュリティポリシーで外部 AI モデルの利用可否を確認する
- GPT-5.4 の効果を実感できるシナリオ：複数ファイルにまたがるバグ修正、大規模リファクタリング、複雑なアーキテクチャ設計相談

#### 参考リンク

- [GPT-5.4 is generally available in GitHub Copilot](https://github.blog/changelog/2026-03-05-gpt-5-4-is-generally-available-in-github-copilot)
- [GitHub Copilot supported models](https://docs.github.com/copilot/reference/ai-models/supported-models)

---

*レポート生成日: 2026年3月14日 / 対象バージョン: VS Code v1.111 / 情報源: VS Code 公式リリースノート・GitHub Copilot Changelog*
