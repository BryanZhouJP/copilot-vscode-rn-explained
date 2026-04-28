# VS Code × GitHub Copilot アップデートレポート: v1.116

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.116
> **リリース日**: 2026年4月15日
> **生成日**: 2026年4月28日
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
| ★★★ | GitHub Copilot の VS Code 内蔵化 | 新規メンバーのオンボーディング時に拡張機能インストール手順を省略 | 導入障壁の撤廃・即日利用開始 |
| ★★★ | フォアグラウンドターミナルへのエージェントアクセス | 実行中の REPL や対話型スクリプトにエージェントが直接介入して自動化 | 手動介入ゼロのエージェント駆動オペレーション |
| ★★★ | エージェントネットワークアクセスのグループポリシー制御 | 組織ポリシーでエージェントのアクセス先ドメインを許可リスト管理し情報漏洩を防止 | エンタープライズセキュリティ要件への適合 |
| ★★★ | バックグラウンドターミナル通知のデフォルト有効化 | デプロイコマンドの完了・入力待ちをチャット上で把握しながら別作業を並行 | 監視作業の省力化・エージェントタスク並列化 |
| ★★★ | 過去エージェントセッションのデバッグログ閲覧 | カスタム指示や Chat カスタマイズのデバッグを過去セッションのログで遡り調査 | 設定最適化の効率化 |
| ★★★ | ターミナル入力処理の大幅改善 | LLM 呼び出し削減と入力質問カルーセルで対話型コマンドの自動化精度が向上 | エージェント処理速度向上・トークン節約 |
| ★★☆ | カスタマイゼーションウェルカムページ | 自然言語の説明から VS Code がエージェント・スキル・指示ファイルを自動ドラフト | カスタム指示整備の工数削減 |
| ★★☆ | Copilot CLI での思考努力量の設定 | 複雑なインフラ設計には高思考量モデルを、日常タスクは低思考量モデルを選択 | コスト・品質のバランス最適化 |
| ★★☆ | ツール確認カルーセル（実験的） | 複数ツール呼び出しをカルーセルで一括レビュー・承認し、会話を止めずに継続 | エージェント操作の確認効率向上 |
| ★☆☆ | Edit Mode 廃止予定（v1.125で完全削除） | v1.110 で廃止通知済みの Edit Mode が v1.125 で設定不可になることを把握 | 廃止前の移行準備を計画的に実施 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. GitHub Copilot の VS Code 内蔵化

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 動作変更 |
| 対象コンポーネント | Copilot Chat / コード補完 / Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**GitHub Copilot Chat が VS Code の組み込み拡張機能になり、新規ユーザーは拡張機能のインストール不要で Copilot 機能（Chat・インライン補完・Agent Mode）を即時利用できるようになった。**

- v1.116 以降の新規インストール VS Code には Copilot が最初から組み込まれている
- 既存ユーザーの動作は変更なし。既インストール済み拡張機能はそのまま継続使用可能
- AI 機能を無効にしたい場合は `chat.disableAIFeatures` 設定で制御可能
- 組織の IT 部門がマシンに VS Code を展開する際、Copilot 拡張の別途配布が不要になる

🔗 **情報源**:
- [VS Code 1.116 Release Notes — GitHub Copilot is now built-in](https://code.visualstudio.com/updates/v1_116)

💡 **AI 活用提案**:
- 新規メンバーのオンボーディングマニュアルから「GitHub Copilot 拡張機能のインストール手順」を削除し、VS Code のインストールだけで Copilot が使える状態になったことを周知してください
- グループポリシーや MDM で VS Code を展開している組織では、`chat.disableAIFeatures` 設定の管理方針を IT 部門で決定することを推奨します

---

### 2. フォアグラウンドターミナルへのエージェントツール対応

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**`send_to_terminal` と `get_terminal_output` エージェントツールが、エージェント自身が起動したバックグラウンドターミナルだけでなく、ターミナルパネルに表示されているすべてのフォアグラウンドターミナルでも動作するようになった。**

- 実行中の REPL（Python / Node.js）や対話型スクリプトにエージェントが入力送信・出力読み取りを行える
- 「既に開いているターミナルセッション」を活用した自動化が可能になり、エージェントの操作範囲が大幅拡大
- Windows の PowerShell セッションや Git Bash セッションにそのまま適用できる

📘 **用語解説**:
- **フォアグラウンドターミナル**: ターミナルパネルに表示されており、ユーザーが直接操作可能な端末セッション

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Foreground terminal support for agent tools](https://code.visualstudio.com/updates/v1_116)

💡 **AI 活用提案**:
- 開いたままの Python スクリプトや対話型シェルセッションにエージェントが自動入力できるようになりました。インフラ確認スクリプトを実行しながら、結果をエージェントにリアルタイム解析させる運用フローを設計してみてください

---

### 3. エージェントネットワークアクセスのグループポリシー制御

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode / MCP |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**管理者がグループポリシーでエージェントツール（fetch ツール・内蔵ブラウザ）のネットワークアクセスを許可・拒否ドメインリストで制御できるようになった。**

- `chat.agent.networkFilter` ポリシーを有効にすると、許可リスト・拒否リストに従ってドメインアクセスを制限
- `chat.agent.allowedNetworkDomains`：アクセス許可ドメイン（ワイルドカード `*.example.com` 対応）
- `chat.agent.deniedNetworkDomains`：アクセス拒否ドメイン（拒否リストが優先）
- 両リストが空で `networkFilter` が有効の場合、全ドメインがブロックされる
- `chat.agent.sandbox.enabled` と併用するとターミナルサンドボックスにも同ルールが適用される
- ポリシーキー: `ChatAgentNetworkFilter`、`ChatAgentAllowedNetworkDomains`、`ChatAgentDeniedNetworkDomains`

📘 **用語解説**:
- **グループポリシー（GP）**: Windows Active Directory 等でソフトウェア設定を組織全体に強制適用する仕組み

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Group policy to filter agent network access](https://code.visualstudio.com/updates/v1_116)
- [VS Code Enterprise Policies ドキュメント](https://code.visualstudio.com/docs/enterprise/policies)

💡 **AI 活用提案**:
- 社内セキュリティポリシーに基づき、エージェントが利用可能な外部ドメインを許可リストで管理することを検討してください。まず `*.github.com` と `*.microsoft.com` を許可リストに追加し、段階的に拡張するアプローチが安全です
- サンドボックス環境での検証時は `chat.agent.networkFilter` を有効にして社内 CI/CD や内部 API のみにアクセスを制限し、意図しない外部通信を防ぐ設定をお試しください

---

### 4. バックグラウンドターミナル通知のデフォルト有効化

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 動作変更 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**エージェントがバックグラウンドターミナルでコマンドを実行する際に、完了・タイムアウト・入力待ちを自動通知する機能（`chat.tools.terminal.backgroundNotifications`）がデフォルト有効になった。**

- エージェントがターミナル出力をポーリングする必要がなくなり、応答速度と精度が向上
- コマンド完了時に即座に次ステップに進めるため、長時間タスクの自動化が効率化される
- 無効にしたい場合は `"chat.tools.terminal.backgroundNotifications": false` を設定

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Background terminal notifications enabled by default](https://code.visualstudio.com/updates/v1_116)

💡 **AI 活用提案**:
- この設定のデフォルト有効化により、以前よりエージェントのターミナル操作が確実になっています。デプロイスクリプトや構成管理ツールの実行をエージェントに委任する場合、この通知機能が正常動作していることを確認してから本番運用に移行することを推奨します

---

### 5. 過去エージェントセッションのデバッグログ閲覧

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode / Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**Agent Debug Log パネルで現在のセッションに加え、ディスクに保存された過去セッションのイベントログも閲覧できるようになった。**

- セッション終了後でも過去のエージェント操作（ツール呼び出し・プロンプト処理）を遡って確認可能
- カスタム指示・エージェントスキル・MCP の動作デバッグ効率が向上
- 設定 `github.copilot.chat.agentDebugLog.fileLogging.enabled` で有効化（デバッグパネルの表示と永続ログが統合された設定キーに整理）

📘 **用語解説**:
- **Agent Debug Log**: エージェントが各セッションで受信したプロンプト・呼び出したツール・返した応答を時系列で記録するパネル

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Debug previous agent sessions](https://code.visualstudio.com/updates/v1_116)
- [Agent Debug Logs パネル ドキュメント](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view#_agent-debug-log-panel)

💡 **AI 活用提案**:
- カスタム指示やスキル定義を調整した際に「なぜ期待どおりに動かなかったか」を過去ログで振り返ることができます。Chat カスタマイズの改善サイクルを高速化するために、デバッグログを常時有効にした開発環境を整備してみてください

---

### 6. ターミナル入力処理の大幅改善

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**ターミナル入力の LLM 検出呼び出しの廃止・プログレスメッセージの改善・Focus Terminal ボタンの追加など、エージェントのターミナル操作体験が包括的に改善された。**

- **LLM 検出呼び出しの廃止**: ターミナル出力ごとに実行されていた「入力待ち判定」の LLM 呼び出しを廃止。レイテンシ削減とトークン節約を実現
- **プログレスメッセージの改善**: 例「Sending "my-project" to terminal (replying to: What is your project name?)」のように、何の質問に回答しているか明示
- **Focus Terminal ボタン**: 入力カルーセルに「Focus Terminal」ボタンが追加され、パスワードや対話型インストーラーへの直接入力が容易になった
- ターミナルに直接タイプし始めた場合はカルーセルが自動的に閉じてユーザー入力を優先する

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Terminal input improvements](https://code.visualstudio.com/updates/v1_116)

💡 **AI 活用提案**:
- LLM 検出呼び出しの廃止によりエージェントのターミナル処理が高速化されています。`npm init` や `pip install` の対話型インストーラーをエージェントに実行させる際の応答速度が改善されているか実際に試してみてください

---

### 7. カスタマイゼーションウェルカムページ

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat / Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**Chat カスタマイズダイアログに新しいウェルカムページが追加され、カスタムエージェント・スキル・指示ファイルの全体像を一覧表示できるようになった。**

- 「Customize Your Agent」入力欄に自然言語で説明すると、VS Code がカスタマイズファイルのドラフトを自動生成
- コマンドパレット（`Ctrl+Shift+P`）→ `Chat: Open Customizations` またはチャットビューのギアアイコンからアクセス
- カスタム指示・エージェント・スキルの作成経験がないユーザーも始めやすくなった

📘 **用語解説**:
- **Chat カスタマイズ**: `.instructions.md`、`.prompt.md`、`.agent.md` などのファイルでエージェントの動作・知識・スキルを定義する仕組み

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Customizations welcome page](https://code.visualstudio.com/updates/v1_116)
- [エージェントカスタマイズ ドキュメント](https://code.visualstudio.com/docs/copilot/customization/overview)

💡 **AI 活用提案**:
- 「インフラ運用チーム向けのカスタムエージェントを作りたい」と自然言語で入力してドラフトを生成させてみてください。生成されたファイルを土台に、チーム固有のナレッジや手順書を追記する方法が効率的です

---

### 8. Copilot CLI での思考努力量の設定

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**Copilot CLI セッションでも推論モデルの思考努力量（thinking effort）をモデルピッカーから設定できるようになった。**

- 推論モデルを選択後、矢印を選択するとサブメニューで努力量レベルを選択可能
- 思考努力量が高いほど回答品質が向上するが、レイテンシも増加するトレードオフ
- 非推論モデルにはサブメニューが表示されない
- VS Code Agents アプリ（Insiders）でも同様に設定可能

📘 **用語解説**:
- **思考努力量（Thinking Effort）**: 推論モデルが回答生成前に内部で推論に費やす計算量のレベル。高くするほど精度が上がるがコスト・時間も増加

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Configure thinking effort in Copilot CLI](https://code.visualstudio.com/updates/v1_116)
- [Thinking and Reasoning ドキュメント](https://code.visualstudio.com/docs/copilot/concepts/language-models#_thinking-and-reasoning)

💡 **AI 活用提案**:
- 複雑なアーキテクチャ設計やトラブルシューティングは高思考量、日常的なスクリプト生成や質問は低思考量と使い分けることで、品質とコストのバランスを最適化することを検討してください

---

### 9. ツール確認カルーセル（実験的）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**複数のツール呼び出し確認をカルーセル（スライド型UI）でまとめてレビュー・承認できる実験的機能が追加された（VS Code Insiders でデフォルト有効、Stable に段階的展開中）。**

- 設定 `chat.tools.confirmationCarousel.enabled` で有効化
- ツール呼び出しごとにスクロールする手間が省け、複数ツールの確認作業がコンパクトに
- 実験的段階のため今後の仕様変更に注意

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Tool confirmation carousel](https://code.visualstudio.com/updates/v1_116)

💡 **AI 活用提案**:
- 複数ファイルの編集や複数ツールを組み合わせたエージェント操作をよく行う場合に有効化を試してみてください。Stable への展開が完了したタイミングで本格導入を検討することを推奨します

---

### 10. Edit Mode 廃止予定（v1.125 で完全削除）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.116（2026年4月15日） |
| カテゴリ | 廃止予定 |
| 対象コンポーネント | Copilot Edits |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**v1.110 で廃止通知された Edit Mode が v1.125 で完全削除される予定。`chat.editMode.hidden` 設定によるリストアは v1.125 まで有効だが、それ以降は機能そのものが削除される。**

- 現時点で Edit Mode を利用している場合は Agent Mode または Copilot Edits への移行計画を立てる必要がある
- `chat.editMode.hidden` 設定は v1.124 以前のバージョンまで有効

🔗 **情報源**:
- [VS Code 1.116 Release Notes — Upcoming deprecations](https://code.visualstudio.com/updates/v1_116)

💡 **AI 活用提案**:
- Edit Mode を使用している開発者や手順書が残っている場合は、v1.125 リリース前に Agent Mode または Copilot Edits へ移行してください。移行後の動作確認を早めに実施することを推奨します

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: GitHub Copilot の VS Code 内蔵化

#### 機能詳細解説

- **なぜ重要か**: これまで Marketplace から手動インストールが必要だった GitHub Copilot Chat が VS Code 本体に組み込まれた。組織展開の工程が簡略化され、新規ユーザーは VS Code を起動してサインインするだけで AI 機能が使える
- **何が変わるか**: 新規インストールの VS Code には最初から Copilot が含まれている。既存ユーザーの既インストール拡張機能の動作は変わらない
- **管理者視点**: `chat.disableAIFeatures` 設定や MDM ポリシーで AI 機能を無効化できるため、AI 利用を制限したい環境でも管理可能
- **セキュリティ**: 組み込み拡張機能になったことで、今後のセキュリティパッチが VS Code 本体のアップデートと一体化される。バージョン管理が一元化されるメリットがある
- **コスト**: 内蔵化自体は無償。利用には引き続き GitHub Copilot サブスクリプション（または GitHub アカウントの無料枠）が必要

#### 前提条件

- **VS Code バージョン**: 1.116 以上（新規インストールの場合）
- **GitHub Copilot プラン**: 利用開始には GitHub アカウントが必要（Free/Pro/Business/Enterprise いずれか）
- **必要な設定**: VS Code へのサインイン（`Ctrl+Shift+P` → `GitHub: Sign in`）
- **OS**: Windows（本ガイドは Windows 環境での手順を前提とする）

#### セットアップ手順

1. **VS Code 1.116 以上をインストール（新規インストールの場合）**
   ```
   https://code.visualstudio.com/ からインストーラーをダウンロード
   ```
   既存ユーザーは VS Code のアップデートのみで動作変更はない

2. **GitHub アカウントでサインイン**
   - VS Code 左下のアカウントアイコンをクリック
   - 「GitHub でサインイン」を選択してブラウザで認証

3. **Copilot が利用可能になったことを確認**
   - `Ctrl+Alt+I` で Copilot Chat パネルが開くことを確認
   - インライン補完は任意のコードファイルを開いてタイプすると動作

> 💡 **ヒント**: 既存メンバーで GitHub Copilot 拡張機能をインストール済みの場合は動作変更なし。新規メンバーは拡張機能インストール手順をスキップできる。
> - コマンドパレット: `Ctrl+Shift+P` → `GitHub: Sign in`
> - 設定 UI: ファイル → 基本設定 → 設定 → `chat.disableAIFeatures`

#### 使い方（ハンズオン）

**シナリオ**: 新しくチームに加わったメンバーが VS Code を初めてインストールし、当日から Copilot Chat でコードレビューを行いたい。従来は拡張機能のインストール・有効化・ライセンス確認などの追加手順が必要だった。

**この機能がない場合**:
- Marketplace から「GitHub Copilot」と「GitHub Copilot Chat」を別途検索してインストールする必要がある
- ライセンス確認・拡張機能の有効化・再起動など複数ステップが必要で 5〜10 分かかる
- 拡張機能バージョンと VS Code バージョンの組み合わせ問題が生じることがある

**操作手順**:

1. VS Code 1.116 以上を `code.visualstudio.com` からダウンロードしてインストール
2. VS Code を起動し、左下アカウントアイコン → 「GitHub でサインイン」
3. ブラウザで GitHub 認証を完了する
4. VS Code に戻り `Ctrl+Alt+I` で Chat パネルを開く
5. 「このリポジトリのコーディング規約を日本語でまとめて」と入力して送信

**期待される結果**:
- 拡張機能のインストールなしで Copilot Chat が即座に応答する
- インライン補完も標準で有効になっており、コードファイルを開いてタイプすると候補が表示される
- オンボーディング時間が大幅短縮され、当日から Copilot を業務に活用できる

#### ベストプラクティス（IT インフラ業務向け）

- **組織展開**: Windows の MDM（Intune 等）で VS Code を配布している場合は、`chat.disableAIFeatures: true` を初期設定に含め、Copilot ライセンスが付与されたユーザーのみが有効化できるポリシーを検討する
- **バージョン管理**: Copilot が内蔵化されたことで、VS Code のバージョン管理 = Copilot のバージョン管理になる。組織の VS Code バージョン更新ポリシーに Copilot の変更点確認を組み込むことを推奨する
- **既存ユーザーへの通知**: Copilot 拡張機能のみをアンインストールしても Copilot 機能が残るため、意図的に無効化したい場合は `chat.disableAIFeatures` 設定を使う必要があることを周知する

#### 参考リンク

- [VS Code 1.116 Release Notes — GitHub Copilot is now built-in](https://code.visualstudio.com/updates/v1_116)
- [GitHub Copilot 入門ドキュメント](https://docs.github.com/en/copilot)
- [VS Code Enterprise Policies](https://code.visualstudio.com/docs/enterprise/policies)

---

### 習得ガイド 2: フォアグラウンドターミナルへのエージェントアクセス

#### 機能詳細解説

- **なぜ重要か**: エージェントがアクセスできるターミナルが「エージェント自身が起動したバックグラウンドターミナル」から「ターミナルパネルに表示されているすべてのターミナル」に拡大された。これによりユーザーが手動で開いているセッションをエージェントが直接操作できる
- **何が変わるか**: `send_to_terminal` ツールで既存の PowerShell セッション・Python REPL・SSH セッションに入力を送信できる。`get_terminal_output` ツールで出力を読み取ってエージェントが判断できる
- **業務での効果**: インフラ確認コマンドの実行とその結果分析を同一セッションで完結できる。SSH で接続中のサーバーターミナルにエージェントがコマンドを送信する活用も可能
- **注意点**: エージェントがフォアグラウンドターミナルに書き込む際は確認を求めるプロンプトが表示される（ユーザー許可が必要）

#### 前提条件

- **VS Code バージョン**: 1.116 以上
- **GitHub Copilot プラン**: Copilot が有効なプランであれば利用可能
- **必要な設定**: Agent Mode が有効（デフォルト有効）
- **OS**: Windows（PowerShell / Git Bash 等のターミナルセッションが対象）

#### セットアップ手順

1. **Agent Mode でチャットを開く**
   ```
   Ctrl+Alt+I でチャットパネルを開き、入力欄のモードを「Agent」に切り替え
   ```

2. **フォアグラウンドターミナルを開く**
   - `Ctrl+` でターミナルパネルを開き、PowerShell や Git Bash のセッションを起動
   - ターミナルに任意のプロセス（REPL、対話型スクリプト等）を起動しておく

3. **エージェントにターミナルへのアクセスを指示する**
   ```
   例：「開いているターミナルで 'Get-Process | Where-Object CPU -gt 50' を実行して、
       CPU 使用率の高いプロセスを分析してください」
   ```

> 💡 **ヒント**: エージェントにフォアグラウンドターミナルへの書き込みを許可するか確認ダイアログが表示されます。内容を確認してから「許可」してください。

#### 使い方（ハンズオン）

**シナリオ**: サーバーの状態確認スクリプトを PowerShell で実行しながら、エージェントにその結果を解析させて問題のある点をレポートしてもらいたい。手動でコマンド出力をコピーしてチャットに貼り付ける手間を省きたい。

**この機能がない場合**:
- ターミナルでコマンドを実行 → 出力をコピー → Chat に貼り付けて質問という 3 ステップが必要
- 大量の出力をコピーして貼り付けるときにコンテキストが切れたり、文字数制限に引っかかる場合がある
- エージェントが判断結果に基づいて追加コマンドを実行しようとしても、手動操作が毎回必要

**操作手順**:

1. ターミナルで PowerShell セッションを開く（`Ctrl+`）
2. Chat パネルを Agent モードで開く（`Ctrl+Alt+I`）
3. 「開いている PowerShell ターミナルで `Get-EventLog -LogName System -EntryType Error -Newest 10` を実行して、エラーを日本語で要約して」と入力
4. エージェントがターミナルアクセスの許可を求めるので「許可」する
5. コマンド実行 → 出力読み取り → 分析レポートが Chat に表示される

**期待される結果**:
- エージェントがフォアグラウンドターミナルに自動入力し、出力結果をそのまま解析できる
- システムイベントログのエラー一覧が日本語で分析・要約されて Chat に返ってくる
- 追加調査が必要な場合は次のコマンドも自律的に実行して深掘りできる

#### ベストプラクティス（IT インフラ業務向け）

- **SSH セッションとの組み合わせ**: VS Code のターミナルで SSH 接続したサーバーにエージェントを経由してコマンドを実行する活用が考えられる。実行内容の確認ダイアログで必ず内容をチェックしてから許可する
- **許可管理**: 機密データを扱うターミナルへのアクセス許可は慎重に行う。不必要なアクセスは拒否してエージェントの操作範囲を制限する運用を徹底する

#### 参考リンク

- [VS Code 1.116 Release Notes — Foreground terminal support for agent tools](https://code.visualstudio.com/updates/v1_116)
- [VS Code Agent Mode ドキュメント](https://code.visualstudio.com/docs/copilot/agents/agent-mode)

---

### 習得ガイド 3: エージェントネットワークアクセスのグループポリシー制御

#### 機能詳細解説

- **なぜ重要か**: エージェントツール（fetch ツール・内蔵ブラウザ）がアクセスできるネットワークドメインを管理者が一元制御できる。情報漏洩リスクの高い外部ドメインへのアクセスをポリシーでブロックできる
- **何が変わるか**: Windows のグループポリシー（GPO）または Intune でポリシーを配布すると、組織内の全 VS Code に対してネットワークフィルターが適用される
- **構成**: 許可リスト（`ChatAgentAllowedNetworkDomains`）と拒否リスト（`ChatAgentDeniedNetworkDomains`）を組み合わせて制御。拒否リストが優先される
- **適用範囲**: `chat.agent.sandbox.enabled` と組み合わせると、ターミナルサンドボックス内のプロセスにも同ルールが適用される

#### 前提条件

- **VS Code バージョン**: 1.116 以上
- **GitHub Copilot プラン**: Business または Enterprise（管理者ポリシー設定が必要）
- **必要な設定**: Windows グループポリシー管理コンソール（GPMC）または Intune での設定
- **OS**: Windows（Active Directory / Azure AD 環境）

#### セットアップ手順

1. **VS Code グループポリシーテンプレートを取得する**
   ```
   VS Code ポリシーファイルの場所:
   C:\Program Files\Microsoft VS Code\resources\app\out\vs\workbench\contrib\policies\
   ```
   または公式ドキュメントからポリシー定義を参照する

2. **グループポリシーオブジェクトを作成する**
   ```
   GPMC（グループポリシー管理コンソール）で新しい GPO を作成
   → コンピューターの構成 → 管理用テンプレート → VS Code
   → GitHub Copilot → ChatAgentNetworkFilter を「有効」に設定
   ```

3. **許可・拒否ドメインリストを設定する**
   ```
   ChatAgentAllowedNetworkDomains:
   *.github.com
   *.microsoft.com
   *.azure.com
   <社内ドメイン>.example.com

   ChatAgentDeniedNetworkDomains:
   *.unknown-domain.com
   ```

4. **GPO を対象 OU に適用して動作を確認する**
   - テスト端末で VS Code を再起動
   - Agent Mode で fetch ツールを使って許可ドメインと拒否ドメインへのアクセスを各々試験

> 💡 **ヒント**: まず「全ドメインを許可リストのみで管理する方式」（空の拒否リスト + 明示的な許可リスト）か、「禁止ドメインのみ拒否リストで管理する方式」かをポリシーの設計段階で決定してください。

#### 使い方（ハンズオン）

**シナリオ**: 組織内の VS Code Copilot Agent に社外の無認可 API やデータ収集サービスへのアクセスを禁止しつつ、GitHub や Azure などの業務に必要な外部ドメインへのアクセスは許可するセキュリティポリシーを設定したい。

**この機能がない場合**:
- エージェントが fetch ツールや内蔵ブラウザを使って任意の外部サイトにアクセスできる
- 機密コードや設定情報が意図せず外部サービスに送信されるリスクを防ぐ手段がなかった
- 利用者個人の判断に委ねるしかなく、組織として統一的なセキュリティ管理ができなかった

**操作手順**:

1. GPMC で新しい GPO「VS Code Agent Network Policy」を作成
2. `ChatAgentNetworkFilter` を「有効」に設定
3. `ChatAgentAllowedNetworkDomains` に `*.github.com`、`*.microsoft.com`、`*.azure.com` を追加
4. `ChatAgentDeniedNetworkDomains` は空白のまま（許可リスト外は全ブロック）
5. 開発部門の OU に GPO を適用してテスト端末で検証
6. Agent Mode で `https://github.com/microsoft/vscode` にアクセスするタスクを実行 → 成功することを確認
7. Agent Mode で許可リスト外のドメインにアクセスするタスクを実行 → ブロックされることを確認

**期待される結果**:
- 許可ドメイン（GitHub、Microsoft、Azure）へのエージェントアクセスは正常に動作する
- 拒否・未許可ドメインへのアクセスはポリシーによりブロックされる
- ユーザーは設定変更できず、組織全体で統一されたネットワークアクセス制御が維持される

#### ベストプラクティス（IT インフラ業務向け）

- **段階的な適用**: 最初は監査モードで動作を観察してから制限を強化するアプローチを取る。開発者の業務に必要なドメインを把握してから許可リストを確定する
- **ドキュメント管理**: 許可ドメインリストは Git 管理して変更履歴を残す。セキュリティレビューのタイミングで定期的に見直す
- **サンドボックスとの組み合わせ**: `chat.agent.sandbox.enabled` と組み合わせることでターミナルプロセスにも同ルールを適用でき、より堅牢な隔離環境を実現できる

#### 参考リンク

- [VS Code 1.116 Release Notes — Group policy to filter agent network access](https://code.visualstudio.com/updates/v1_116)
- [VS Code Enterprise Policies ドキュメント](https://code.visualstudio.com/docs/enterprise/policies)
- [GitHub Copilot Enterprise セキュリティ設定](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization)

---

*レポート生成日: 2026年4月28日 | 情報源: VS Code 1.116 Release Notes, GitHub Changelog (April 2026)*
