# VS Code × GitHub Copilot アップデートレポート: v1.117

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.117
> **リリース日**: 2026年4月22日
> **生成日**: 2026年4月28日
> **対象読者**: クラウド・オンプレ基盤の企画・開発・運用を担当する IT 技術者（Windows + VS Code + GitHub Copilot 環境）

---

## 目次

- [新機能・変更一覧](#新機能変更一覧)
  - [AI活用提案サマリー](#ai活用提案サマリー)
- [習得ガイド（推奨レベル順 上位3件）](#習得ガイド)

---

## 新機能・変更一覧

> 8 件のアップデートを業務活用推奨レベル順に紹介します。

### AI活用提案サマリー

今回の VS Code × GitHub Copilot アップデートの中で、**クラウド・オンプレ基盤担当の IT 技術者**が特に注目すべき活用シナリオをまとめます。

| 推奨 | 機能名 | 活用シナリオ | 期待効果 |
| ---- | ------ | ----------- | -------- |
| ★★★ | BYOK（自前 API キー接続） | 組織で契約済みの Azure OpenAI や Anthropic キーを Copilot Chat に接続し、社内モデルを流用 | モデル利用コストの一元管理・Copilot クォータ節約 |
| ★★★ | バックグラウンドターミナル通知 | 長時間の Infrastructure as Code デプロイコマンドをエージェントに任せ、完了通知を受け取りながら別作業 | 監視作業の手間削減・エージェントタスクの並列化 |
| ★★★ | カスタムシェルからの Copilot CLI 起動 | Git Bash / PowerShell 既定環境でも Copilot CLI を直接開き、インフラタスクのエージェント操作を開始 | 環境依存のエラーを排除し、CLI エージェント活用のハードルを低減 |
| ★★★ | エージェント CLI のターミナルタイトル自動表示 | 複数タブで Copilot CLI・Claude Code・Gemini CLI を並走させるときの視認性向上 | マルチエージェント運用時の誤操作防止 |
| ★★★ | セッションの最終更新順ソート | 大量の過去エージェントセッションを素早く再開 | セッション管理効率向上 |
| ★★★ | GPT-5.5 一般提供（GA） | 複雑なインフラ設計・障害解析など高難度タスクに最新モデルを適用 | 推論精度向上によるレビュー工数削減 |
| ★★☆ | チャット応答のインクリメンタルレンダリング（実験的） | 長大なコード生成や設計ドキュメント生成時のUX改善 | 待機感を軽減しストリーミングレスポンスの視認性向上 |
| ★★☆ | VS Code Agents アプリ更新（Insiders） | サブセッションでリファクタ・コードレビューを並列実行 | 複数タスク同時進行による開発速度向上 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. BYOK（自前言語モデル API キー）— Copilot Business / Enterprise 向け GA

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.117（2026年4月22日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**Copilot Business / Enterprise ユーザーが自組織の API キーで任意のモデルを VS Code Chat に接続できるようになった。**

- Anthropic、Google Gemini、OpenAI、OpenRouter、Azure、Ollama、Foundry Local に対応したプロバイダーキーを設定可能
- BYOK 経由の利用は GitHub Copilot のリクエストクォータにカウントされず、費用は各プロバイダーに直接請求される
- コード補完（インライン候補）には適用されず、Chat 機能のみが対象
- 管理者は GitHub.com のポリシー設定で組織全体の BYOK を無効化できる（デフォルトは有効）
- 設定は VS Code の `github.copilot.chat.byok` 関連設定または言語モデルプロバイダー拡張機能のインストールで行う

📘 **用語解説**:
- **BYOK（Bring Your Own Key）**: 自組織が保有する API キーを外部サービスに持ち込んで利用する方式
- **OpenRouter**: 複数の AI プロバイダー API を統一インターフェースで利用できるプロキシサービス
- **Foundry Local**: Microsoft が提供するローカル実行可能な AI モデル実行基盤

🔗 **情報源**:
- [Bring your own language model key in VS Code — GitHub Changelog](https://github.blog/changelog/2026-04-22-bring-your-own-language-model-key-in-vs-code-now-available)
- [VS Code 1.117 Release Notes](https://code.visualstudio.com/updates/v1_117)

💡 **AI 活用提案**:
- 組織で契約済みの Azure OpenAI リソースのキーを登録し、社内コンプライアンスポリシーに準拠したモデルを Copilot Chat で利用することを検討してください
- コスト管理の観点から、BYOK 利用ログを Azure のコスト分析と合わせて月次で確認する運用フローを整備してみてください

---

### 2. バックグラウンドターミナルコマンドのシステム通知

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.117（2026年4月22日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**エージェントがバックグラウンドで実行する長時間ターミナルコマンドの進捗状況が、チャット応答内にシステム通知として表示されるようになった。**

- コマンドの完了・タイムアウト・入力待ちなどのイベントをターミナルに切り替えずにチャット上で確認できる
- デプロイスクリプトや大規模テスト実行など、時間のかかる操作をエージェントに委任した際の見逃し防止に有効
- バックグラウンドターミナル通知設定（`chat.tools.terminal.backgroundNotifications`）は v1.116 から デフォルト有効済み

🔗 **情報源**:
- [VS Code 1.117 Release Notes — System notifications for background terminal commands](https://code.visualstudio.com/updates/v1_117)

💡 **AI 活用提案**:
- Terraform apply や Ansible playbook の実行をエージェントに任せ、通知を受け取りながら他の設計作業を並行して進める運用を試してみてください
- 入力待ち通知を活用して、対話型インストーラーの質問にもチャットから回答できるか確認してみてください

---

### 3. カスタムターミナルプロファイルからの Copilot CLI 起動

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.117（2026年4月22日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**デフォルト以外のシェル（Windows の Git Bash、macOS/Linux の fish など）を既定プロファイルに設定している環境でも Copilot CLI ターミナルが正常起動するようになった。**

- 従来は `No terminal profile options provided for id 'copilot-cli'` エラーでターミナルが起動できなかった
- ターミナルパネルのプロファイルピッカーから「GitHub Copilot CLI」を選択するだけで動作するようになった
- Windows でも Git Bash を既定にしている環境（開発ポリシーで強制されるケースなど）で恩恵が大きい

🔗 **情報源**:
- [VS Code 1.117 Release Notes — Launch Copilot CLI with a custom terminal profile](https://code.visualstudio.com/updates/v1_117)

💡 **AI 活用提案**:
- Git Bash を標準シェルとして統一している開発チームで、Copilot CLI エージェントの導入障壁を取り除けます。CI/CD スクリプト作成タスクへの活用を検討してください

---

### 4. エージェント CLI のターミナルタイトル自動表示

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.117（2026年4月22日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**ターミナルで起動した Copilot CLI・Claude Code・Gemini CLI が `node` ではなく CLI 名でタブタイトルに表示されるようになった。**

- OSC タイトルシーケンスを検出してシェル種別を識別し、適切なタイトルを設定する
- Windows・macOS・Linux に対応。Codex は macOS で OSC シーケンスを出力しないため未対応
- 設定 `terminal.integrated.tabs.allowAgentCliTitle` で無効化可能（デフォルト有効）
- 複数エージェント CLI を並走させる際の誤操作・混乱を防止できる

📘 **用語解説**:
- **OSC タイトルシーケンス**: ターミナルアプリがタブタイトルを設定するためのエスケープシーケンス（`\e]0;タイトル\a`）

🔗 **情報源**:
- [VS Code 1.117 Release Notes — Terminal title for agent CLIs](https://code.visualstudio.com/updates/v1_117)

💡 **AI 活用提案**:
- Copilot CLI と Claude Code を用途別に複数ターミナルで並走させる運用が安全に行えます。チームの CLI エージェント活用ガイドラインにターミナルタイトルによる管理方法を追記することを検討してください

---

### 5. エージェントセッションの最終更新順ソート

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.117（2026年4月22日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat / Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**Agent Sessions ビューでセッションを「作成日時」または「最終更新日時」でソートできるようになった。**

- セッションが多数蓄積した際に作業継続セッションをすぐに見つけられる
- フィルターコンテキストメニューから「Sort by updated」または「Sort by created」を選択
- 長期にわたるインフラ改修や複数プロジェクト並走時のセッション管理に有効

🔗 **情報源**:
- [VS Code 1.117 Release Notes — Sort agent sessions by recent activity](https://code.visualstudio.com/updates/v1_117)

💡 **AI 活用提案**:
- 週次でインフラ作業ごとにエージェントセッションを作成し、最終更新順で管理する習慣をチームに定着させることを検討してください

---

### 6. GPT-5.5 の一般提供（GA）開始

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | GitHub Copilot（2026年4月24日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat / コード補完 |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**GPT-5.5 が GitHub Copilot で一般提供（GA）され、モデルピッカーから選択可能になった。**

- Preview から GA になり安定稼働・SLA が適用される段階に移行
- 複雑なアーキテクチャ設計・大規模コードリファクタリング・障害調査など高難度タスクへの適用が推奨
- モデルピッカーから GPT-5.5 を選択するだけで即時利用可能

🔗 **情報源**:
- [GPT-5.5 is generally available for GitHub Copilot — GitHub Changelog](https://github.blog/changelog/2026-04-24-gpt-5-5-is-generally-available-for-github-copilot)

💡 **AI 活用提案**:
- 既存の Terraform モジュールや Ansible ロールのレビュー・最適化提案タスクに GPT-5.5 を試用し、GPT-5.3 との回答品質を比較評価してみてください

---

### 7. チャット応答のインクリメンタルレンダリング（実験的）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.117（2026年4月22日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**チャット応答をブロック単位でアニメーション付きにストリーミング表示する実験的機能が追加された。**

- `chat.experimental.incrementalRendering.enabled`（デフォルト `true`）で制御
- アニメーションスタイルは `fade`/`rise`/`blur`/`scale`/`slide`/`reveal`/`none` から選択
- バッファリング設定（`off`/`word`/`paragraph`）で表示速度と表示品質をトレードオフ調整
- 実験的段階のため今後の設定キー・動作変更に注意

🔗 **情報源**:
- [VS Code 1.117 Release Notes — Incremental rendering of chat responses](https://code.visualstudio.com/updates/v1_117)

💡 **AI 活用提案**:
- 長大なコード生成や設計ドキュメントの生成でレスポンス待機感を改善したい場合に試してみてください。GA 後に標準化されることを見据えて動作を確認しておくことを推奨します

---

### 8. VS Code Agents アプリ更新（Insiders プレビュー）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code Insiders 1.117（プレビュー） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**VS Code Agents コンパニオンアプリにサブセッション作成機能とインライン差分レンダリング改善が追加された（Insiders 限定）。**

- セッションタイトルの `+` ボタンで現在のコンテキストを引き継いだサブセッションを生成できる
- コードレビューやリサーチを並行して行いながら親セッションの作業を継続可能
- インライン変更レンダリングが改善されエージェントの編集内容をスキャンしやすくなった
- Stable リリースへの昇格時期は未定のため現時点は評価・検証段階での利用を推奨

🔗 **情報源**:
- [VS Code 1.117 Release Notes — Visual Studio Code Agents (Insiders)](https://code.visualstudio.com/updates/v1_117)

💡 **AI 活用提案**:
- VS Code Insiders をサンドボックス環境に導入し、大規模インフラ変更作業でサブセッションを使ったエージェント並列化を先行評価してみてください

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: BYOK（自前言語モデル API キー接続）— Copilot Business / Enterprise

#### 機能詳細解説

- **なぜ重要か**: 組織が契約・管理している Azure OpenAI や Anthropic のモデルを Copilot Chat に接続できる。これにより「社内モデルは使いたいが、ツールを Copilot から切り替えたくない」という課題を解消する
- **何が変わるか**: Chat ウィンドウのモデルピッカーに BYOK 登録したモデルが表示され、そのまま会話・コード生成に使える。Copilot のリクエストクォータは消費されず、費用はプロバイダーへの直接課金になる
- **どんな場面で使うか**: コンプライアンス要件でデータを Azure テナント外に出せない場合に Azure OpenAI の BYOK を使う、または特定のドメイン特化モデルを Ollama でローカル実行してコード補完以外の Chat に適用するケースが代表的
- **注意点**: BYOK はコード補完（インライン候補）には使えない。Chat のみが対象。管理者がポリシーで無効化している場合は利用不可

#### 前提条件

- **VS Code バージョン**: 1.117 以上
- **GitHub Copilot プラン**: Business または Enterprise（Individual は対象外）
- **必要な設定**: GitHub.com の組織設定でポリシーが有効（デフォルト有効）であること
- **OS**: Windows（本ガイドは Windows 環境での手順を前提とする）

#### セットアップ手順

1. **モデルピッカーを開く**
   ```
   チャット入力欄左側のモデルアイコンをクリック → 「+ Add Model...」を選択
   ```
   またはコマンドパレット（`Ctrl+Shift+P`）→ `Chat: Add Language Model` を実行

2. **プロバイダーを選択してキーを入力する**
   - 表示されるプロバイダー一覧（Anthropic、OpenAI、Azure、OpenRouter、Ollama、Foundry Local 等）から対象を選択
   - API キーまたはエンドポイント URL を入力して「Save」をクリック

3. **登録したモデルを選択して Chat を開始する**
   ```
   モデルピッカーから登録済みモデル名を選択
   ```
   以降の Chat 会話がそのモデルを使って処理される

> 💡 **ヒント**: Azure OpenAI を使う場合は、エンドポイント URL（`https://<リソース名>.openai.azure.com/`）とデプロイ名を準備してから操作してください。
> - コマンドパレット: `Ctrl+Shift+P` → `Chat: Add Language Model`
> - 設定 UI: ファイル → 基本設定 → 設定 → `github.copilot.chat.byok`

#### 使い方（ハンズオン）

**シナリオ**: 組織が Azure で管理している GPT-4o デプロイメントを、プライベートな構成ドキュメント生成タスクに使用したい。社外 API への送信を避けるため Copilot デフォルトモデルが利用できないが、Azure OpenAI なら社内のコンプライアンスポリシーに適合している。

**この機能がない場合**:
- Azure OpenAI Studio または別ツールで問い合わせを行い、VS Code に結果を手動コピーする必要がある
- ツール間の行き来による作業中断とコンテキスト切り替えコストが発生する
- コードとドキュメントを同一セッションで生成・参照することができない

**操作手順**:

1. Copilot Chat パネルを開く（`Ctrl+Alt+I`）
2. モデルアイコン（入力欄左）をクリック → 「+ Add Model...」を選択
3. 「Azure OpenAI」を選択し、エンドポイント URL・デプロイ名・API キーを入力
4. モデルピッカーで登録した Azure GPT-4o デプロイを選択
5. チャット入力欄に「この Terraform ファイルのセキュリティ設定を日本語で説明して」と入力して送信

**期待される結果**:
- Azure テナント上の GPT-4o デプロイがリクエストを処理し、VS Code Chat に回答が表示される
- Copilot のリクエストカウンターは増加しない（Azure 側の課金のみが発生）
- 同一 Chat セッション内で コード参照・質問・ドキュメント生成が完結する

#### ベストプラクティス（IT インフラ業務向け）

- **キー管理**: API キーは VS Code の秘密情報ストア（Windows 資格情報マネージャー）に暗号化保存される。シェアPCやCI環境への直接入力は避け、環境変数やシークレットマネージャー経由での設定を検討する
- **プロバイダー選定**: コンプライアンス要件が厳しい環境は Azure OpenAI（データが Azure テナント内で処理される）を優先する。実験・評価目的には Ollama を使ったローカルモデルも有効
- **管理者向け**: 組織で BYOK 利用を制御する場合は `github.com/settings/copilot/features` の「Bring Your Own Language Model Key」ポリシーをチームの要件に合わせて設定する

#### 参考リンク

- [Bring your own language model key — VS Code ドキュメント](https://code.visualstudio.com/docs/copilot/customization/language-models#_bring-your-own-language-model-key)
- [GitHub Changelog: BYOK in VS Code now available](https://github.blog/changelog/2026-04-22-bring-your-own-language-model-key-in-vs-code-now-available)
- [VS Code 1.117 Release Notes](https://code.visualstudio.com/updates/v1_117)

---

### 習得ガイド 2: バックグラウンドターミナルコマンドのシステム通知

#### 機能詳細解説

- **なぜ重要か**: エージェントがバックグラウンドで実行するターミナルコマンドの状態変化（完了・タイムアウト・入力待ち）をチャット上で確認できる。ターミナルパネルへの切り替えが不要になり、エージェントタスクと会話の流れを中断せずに管理できる
- **何が変わるか**: バックグラウンドターミナルでのコマンド完了時に Chat 応答内にシステム通知が表示されるようになる。v1.116 でデフォルト有効になった `chat.tools.terminal.backgroundNotifications` 設定との組み合わせで動作する
- **どんな場面で使うか**: `terraform apply`、`ansible-playbook`、`docker build`、長時間テスト実行などの時間のかかるコマンドをエージェントに実行させながら、別の設計タスクや Chat での質問を並行して進める場面
- **補足**: 入力待ち通知ではクエスチョンカルーセルが表示され「Focus Terminal」ボタンでターミナルに直接アクセスすることも可能

#### 前提条件

- **VS Code バージョン**: 1.117 以上（`backgroundNotifications` は 1.116 からデフォルト有効）
- **GitHub Copilot プラン**: Copilot が有効なプランであれば利用可能
- **必要な設定**: `chat.tools.terminal.backgroundNotifications: true`（デフォルト有効）
- **OS**: Windows

#### セットアップ手順

1. **設定を確認する（デフォルト有効のため通常不要）**
   ```json
   // settings.json
   "chat.tools.terminal.backgroundNotifications": true
   ```
   コマンドパレット（`Ctrl+Shift+P`）→ `Preferences: Open Settings (JSON)` で確認

2. **エージェントモードで Chat を開く**
   - Chat パネル（`Ctrl+Alt+I`）を開き、入力欄上部のモード切替で「Agent」を選択

3. **長時間コマンドを含むタスクを依頼する**
   ```
   例: 「このリポジトリのDockerイメージをビルドして動作確認してください」
   ```

> 💡 **ヒント**: バックグラウンドターミナルを使ってエージェントが実行しているコマンドは、Chat の「ツール実行結果」セクションで常に確認できます。通知が多すぎる場合は設定を `false` に変更してください。

#### 使い方（ハンズオン）

**シナリオ**: エージェントに `ansible-playbook site.yml` を実行させつつ、同じ Chat で設定ファイルの修正を並行して依頼したい。コマンドの完了を待ちながら別の作業をしている最中に、完了したタイミングを見逃したくない。

**この機能がない場合**:
- ターミナルタブに切り替えて手動でコマンドの終了を確認する必要がある
- 画面を頻繁に切り替えることで Chat の文脈が途切れ、作業効率が低下する
- タイムアウトや入力待ちに気づかず、エージェントが停止したまま放置されるリスクがある

**操作手順**:

1. Chat パネルを Agent モードで開く（`Ctrl+Alt+I` → Agent モード選択）
2. 入力欄に「`ansible-playbook site.yml` を実行して、その間に hosts.yml のフォーマットを確認してください」と入力
3. エージェントがバックグラウンドターミナルでコマンドを実行する
4. 並行して Chat で別の質問（`hosts.yml` のレビューなど）を続ける
5. コマンドが完了した時点でチャット応答内にシステム通知が表示される

**期待される結果**:
- ターミナルを直接監視しなくても、コマンド完了・エラー・入力待ちをチャット上で把握できる
- 入力が必要な場合はカルーセル UI または「Focus Terminal」ボタンで対応できる
- エージェントは通知を受け取って次のステップを自律的に進める

#### ベストプラクティス（IT インフラ業務向け）

- **長時間コマンドは積極的にバックグラウンド実行に委ねる**: エージェントはバックグラウンドターミナルの完了を自動で検知して続行できるため、`terraform apply` や Docker ビルドなど待機時間の長い操作に活用する
- **パスワード入力が発生するコマンドに注意**: `sudo` や SSH パスフレーズを求めるコマンドは入力待ち通知が発生する。事前に認証情報を設定しておくかエージェントに `-k` オプション等を指定させるよう指示する

#### 参考リンク

- [VS Code 1.117 Release Notes — System notifications for background terminal commands](https://code.visualstudio.com/updates/v1_117)
- [VS Code Agent Mode ドキュメント](https://code.visualstudio.com/docs/copilot/agents/agent-mode)

---

### 習得ガイド 3: カスタムターミナルプロファイルからの Copilot CLI 起動

#### 機能詳細解説

- **なぜ重要か**: Windows で Git Bash を既定シェルとして設定していた場合、Copilot CLI ターミナルが起動できないバグが解消された。これにより多くのチームで "試せなかった" Copilot CLI エージェントが即日使えるようになる
- **何が変わるか**: ターミナルパネルのプロファイルピッカーに「GitHub Copilot CLI」が常時表示され、既定シェルに関わらず起動できる
- **どんな場面で使うか**: Git Bash 環境でインフラスクリプトの生成・実行・デバッグをエージェントに依頼する場面。Windows のコマンドプロンプトでは動作しないシェルスクリプトを Bash で直接実行させるフローに適している

#### 前提条件

- **VS Code バージョン**: 1.117 以上
- **GitHub Copilot プラン**: Copilot が有効なプランであれば利用可能
- **必要な設定**: Git Bash または任意のシェルがターミナルプロファイルに登録されていること
- **OS**: Windows（Git Bash インストール済み）

#### セットアップ手順

1. **ターミナルパネルを開く**
   ```
   Ctrl+` または表示メニュー → ターミナル
   ```

2. **プロファイルピッカーから Copilot CLI を選択する**
   - ターミナルパネル右上の「∨（新しいターミナル）」ドロップダウンをクリック
   - 一覧から「GitHub Copilot CLI」を選択

3. **Copilot CLI セッションが起動したことを確認する**
   - ターミナルに Copilot CLI のウェルカム画面が表示されることを確認
   - プロンプトに質問を入力してエージェントが応答することを確認

> 💡 **ヒント**: 既定のターミナルプロファイルが Git Bash になっていても問題ありません。ドロップダウン一覧に「GitHub Copilot CLI」が表示されない場合は、VS Code を最新バージョン（1.117 以上）に更新してください。

#### 使い方（ハンズオン）

**シナリオ**: Windows 開発マシンで Git Bash を既定シェルとして使っているが、これまで Copilot CLI が起動エラーになっていた。v1.117 で修正された機能を使い、Bash スクリプトのデバッグ作業をエージェントに依頼したい。

**この機能がない場合**:
- 「No terminal profile options provided for id 'copilot-cli'」エラーが発生し、Copilot CLI ターミナルが起動しない
- 既定シェルを PowerShell などに一時変更する手間が必要か、Copilot CLI の利用を断念するしかなかった

**操作手順**:

1. ターミナルパネルを開く（`Ctrl+`）
2. 右上の `∨` ドロップダウンをクリック
3. 「GitHub Copilot CLI」を選択（Git Bash が既定でも問題ない）
4. CLI が起動したら入力欄に「この deploy.sh スクリプトをレビューして問題点を指摘して」と入力
5. 作業対象ファイルを `@deploy.sh` としてコンテキストに追加して送信

**期待される結果**:
- エラーなく Copilot CLI ターミナルが起動し、エージェントが応答する
- Bash スクリプトのレビュー・修正・実行確認をエージェントと対話形式で行える
- ターミナルタイトルには「GitHub Copilot CLI」が表示され（習得ガイド相乗効果）、他のターミナルと区別できる

#### ベストプラクティス（IT インフラ業務向け）

- **Bash スクリプト作業との親和性が高い**: Git Bash 上で動かす必要があるシェルスクリプト（デプロイ・バックアップ・構成確認）の作成・デバッグに Copilot CLI を積極的に活用する
- **ファイルコンテキストの活用**: `@ファイル名` でスクリプトをコンテキストに渡すと、エージェントが内容を理解した上でより的確な修正提案を行う

#### 参考リンク

- [VS Code 1.117 Release Notes — Launch Copilot CLI with a custom terminal profile](https://code.visualstudio.com/updates/v1_117)
- [GitHub Copilot CLI Getting Started](https://github.blog/ai-and-ml/github-copilot/github-copilot-cli-for-beginners-getting-started-with-github-copilot-cli/)

---

*レポート生成日: 2026年4月28日 | 情報源: VS Code 1.117 Release Notes, GitHub Changelog (April 2026)*
