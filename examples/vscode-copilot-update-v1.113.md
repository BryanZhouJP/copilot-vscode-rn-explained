# VS Code × GitHub Copilot アップデートレポート: v1.113

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.113
> **リリース日**: 2026-03-25
> **生成日**: 2026-04-10
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
| ★★★ | MCP support in Copilot CLI & Claude agents | VS Code で設定した DB・監視系 MCP ツールを Copilot CLI のバッチタスクでも利用 | ツール設定の一元化・再利用 |
| ★★★ | Configurable thinking effort | Terraform 設計レビューには High thinking、スニペット生成には Low で効率的にコスト配分 | レスポンス精度とコストのバランス最適化 |
| ★★☆ | Chat Customizations editor | インストラクション・カスタムエージェント・MCP を 1 画面で管理 | カスタマイズ管理の効率化 |
| ★☆☆ | Agent debug logs for CLI sessions | CLI タスクのデバッグ情報を GUI で確認 | 問題切り分けの高速化 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. MCP support in Copilot CLI & Claude agents — CLI/Claude エージェントへの MCP サーバー対応

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 新機能 |
| 対象コンポーネント | MCP / Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**VS Code に登録した MCP サーバーが Copilot CLI・Claude エージェントセッションでも自動的に利用可能になった。**

- ユーザー設定・ワークスペースの `mcp.json` で定義したサーバーがどちらのセッションでもブリッジされる
- これまではローカルエージェント（チャット内）のみに MCP が有効で、CLI や Claude セッションでは使えなかった
- MCP ツールの設定を一箇所に集約でき、セッション種別ごとの設定管理が不要になる

📘 **用語解説**:
- **Copilot CLI セッション**: `gh copilot` コマンドや VS Code チャットの Copilot CLI モードで動作するエージェントセッション

🔗 **情報源**:
- [VS Code 1.113 Release Notes – MCP support in Copilot CLI & Claude agents](https://code.visualstudio.com/updates/v1_113#_mcp-support-in-copilot-cli-claude-agents)
- [Using MCP servers in VS Code](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)

💡 **AI 活用提案**:
- Prometheus・Grafana・Terraform Cloud などの監視・IaC ツール向け MCP サーバーを VS Code で一度設定し、CLI バッチタスクでもそのまま活用できます
- 夜間の自動インフラレビュータスクを Copilot CLI で実行する際、同じ MCP ツールセットを使えるようになります

---

### 2. Configurable thinking effort in model picker — モデルピッカーから推論量を直接制御

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**Claude Sonnet 4.6 / GPT-5.4 などの推論モデルで、モデルピッカーから Low / Medium / High の推論量を直接選択できるようになった。選択値はモデル別に会話をまたいで保持される。**

- VS Code 設定画面を開かずにピッカーのサブメニューから即座に変更可能
- ピッカーのラベルが「GPT-5.4 · Medium」のように現在の推論量を表示する
- 非推論モデル（例: GPT-4o）にはサブメニューが表示されない
- 従来の `github.copilot.chat.anthropic.thinking.effort` と `github.copilot.chat.responsesApiReasoningEffort` 設定は**廃止済み**（モデルピッカーで管理）

🔗 **情報源**:
- [VS Code 1.113 Release Notes – Configurable thinking effort in model picker](https://code.visualstudio.com/updates/v1_113#_configurable-thinking-effort-in-model-picker)
- [Thinking and reasoning – VS Code docs](https://code.visualstudio.com/docs/copilot/concepts/language-models#_thinking-and-reasoning)

💡 **AI 活用提案**:
- 「システム設計の回答精度が欲しい」場合は High、「定型スクリプトの補完」には Low を使い分けることで、推論コストと速度のトレードオフを意識した使い方ができます
- 廃止設定を `settings.json` に残している場合は今すぐ削除してください

---

### 3. Images preview for chat attachments — チャット添付画像の対話型ビューア

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**チャットに添付した画像やエージェントが生成した画像をクリックして専用のビューアで閲覧・操作できるようになった（デフォルト有効）。**

- 矢印ボタン・キーボード・サムネイルで同一セッション内の全画像を一括ブラウズできる
- 会話ターンごとに画像がグループ化される
- ズーム・パン機能あり（クリックで拡大、スクロールで連続ズーム）
- Explorer ビューの画像ファイルを右クリック → `Open in Images Preview` でフォルダ内全画像を一覧
- 設定：`imageCarousel.chat.enabled`・`imageCarousel.explorerContextMenu.enabled` で独立して制御可能（両方デフォルト有効）

🔗 **情報源**:
- [VS Code 1.113 Release Notes – Images preview for chat attachments](https://code.visualstudio.com/updates/v1_113#_images-preview-for-chat-attachments)

💡 **AI 活用提案**:
- ネットワーク構成図・アーキテクチャ図・スクリーンショットをエージェントとのチャットで分析する際に、返ってきた図を拡大表示して詳細確認できるようになります
- 統合ブラウザが撮影したスクリーンショットをセッション内でまとめて確認するレビューワークフローに活用してください

---

### 4. Use self-signed certificates in the integrated browser — 統合ブラウザでの自己署名証明書対応

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 新機能 |
| 対象コンポーネント | その他 |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**統合ブラウザが自己署名証明書のサイトではロードを拒否していた問題が解消され、証明書を一時的に信頼してページを開けるようになった。**

- 一般ブラウザと同様に「接続が安全でありません」画面から「続行」を選択することで 1 週間だけ信頼できる
- URL バーに「安全でない接続」表示が維持され、右クリックでいつでも信頼を取り消せる
- ローカル HTTPS 開発環境のテストや社内 CA 証明書環境での検証に有効
- 設定変更不要

🔗 **情報源**:
- [VS Code 1.113 Release Notes – Use self-signed certificates in the integrated browser](https://code.visualstudio.com/updates/v1_113#_use-self-signed-certificates-in-the-integrated-browser)
- [Integrated browser documentation](https://code.visualstudio.com/docs/debugtest/integrated-browser)

💡 **AI 活用提案**:
- 社内 HTTPS ダッシュボードや Kubernetes ダッシュボードをローカル開発で確認する際に、外部ブラウザへの切り替え不要で作業ができます

---

### 5. Chat Customizations editor — カスタマイズ一元管理エディタ（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**インストラクション・プロンプトファイル・カスタムエージェント・スキル・MCP マーケットプレイスを 1 つの統合エディタで管理できる Preview 機能が追加された。**

- コマンドパレット（`Ctrl+Shift+P`）→ `Chat: Open Chat Customizations` で起動、またはチャットビューのギアアイコンから開く
- AI による初期コンテンツ生成、構文ハイライト・バリデーション付きのコードエディタを内包
- MCP とプラグインのマーケットプレイスを直接ブラウズして追加可能
- Preview 中であるため今後 UI・動作が変わる可能性がある

🔗 **情報源**:
- [VS Code 1.113 Release Notes – Chat Customizations editor](https://code.visualstudio.com/updates/v1_113#_chat-customizations-editor-preview)
- [Chat Customizations editor docs](https://code.visualstudio.com/docs/copilot/customization/overview#_chat-customizations-editor)

💡 **AI 活用提案**:
- チーム新メンバーのオンボーディング時に、この画面で MCP・カスタムエージェント・インストラクションをまとめてセットアップするガイドとして活用することを検討してください

---

### 6. Nested subagents — サブエージェントのネスト呼び出し対応

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**サブエージェントが別のサブエージェントを呼び出せるようになり、複雑な多段ワークフローをエージェントで自動化できる。**

- `chat.subagents.allowInvocationsFromSubagents` 設定を有効にすることで利用可能
- 無限ループ防止のため意図的にデフォルト無効となっている
- 「調査エージェント → コード生成エージェント → レビューエージェント」のような連鎖タスクに有効

📘 **用語解説**:
- **サブエージェント**: カスタムエージェント定義の中から呼び出せる特化型の AI ワークフロー単位

🔗 **情報源**:
- [VS Code 1.113 Release Notes – Nested subagents](https://code.visualstudio.com/updates/v1_113#_nested-subagents)
- [Using subagents](https://code.visualstudio.com/docs/copilot/agents/subagents)

💡 **AI 活用提案**:
- インフラ設計の「要件整理 → Terraform 生成 → セキュリティレビュー」をそれぞれ専門エージェントとして定義し、ネストで自動化するアーキテクチャを将来的に設計することを検討してください

---

### 7. Forking sessions in Copilot CLI & Claude agents — セッションフォークでアプローチの分岐探索

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**Copilot CLI・Claude エージェントセッションのある時点でセッションをフォーク（複製）して、元の会話を残しながら別アプローチを試せるようになった。**

- Copilot CLI でのフォークは `github.copilot.chat.cli.forkSessions.enabled` で有効化（Experimental）
- Claude エージェントでもフォーク可能
- 複数の設計案を並行検討する際に特に効果的

🔗 **情報源**:
- [VS Code 1.113 Release Notes – Forking sessions in Copilot CLI & Claude agents](https://code.visualstudio.com/updates/v1_113#_forking-sessions-in-copilot-cli-claude-agents)
- [Forking a chat session](https://code.visualstudio.com/docs/copilot/chat/chat-sessions#_fork-a-chat-session)

💡 **AI 活用提案**:
- 「AWS と Azure どちらにサービスを移行するか」を同一コンテキストから2方向で探索し、結果を比較するユースケースに使ってみてください

---

### 8. Agent debug logs for Copilot CLI and Claude CLI sessions — CLI セッションのデバッグログ表示（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.113（2026-03-25） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**Agent Debug Log パネルが Copilot CLI および Claude エージェントのセッションにも対応し、ツール使用状況・サブエージェント判断・レイテンシなどを GUI で確認できるようになった。**

- ローカルエージェントセッションではすでに対応済みで、今回 CLI/Claude セッションにも拡張
- Preview 機能であり今後変更の可能性あり
- デバッグログを有効にするには `github.copilot.chat.agentDebugLog.enabled` が必要

🔗 **情報源**:
- [VS Code 1.113 Release Notes – Agent debug logs for Copilot CLI and Claude CLI sessions](https://code.visualstudio.com/updates/v1_113#_agent-debug-logs-for-copilot-cli-and-claude-cli-sessions-preview)
- [Agent Debug Log panel](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view#_agent-debug-log-panel)

💡 **AI 活用提案**:
- CLI によるバッチ処理のパフォーマンス改善や、期待したツールが呼ばれていない問題の調査に活用することを将来的に検討してください

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: MCP support in Copilot CLI & Claude agents — CLI/Claude エージェントへの MCP 対応

#### 機能詳細解説

- これまで VS Code のローカルエージェントでしか使えなかった MCP サーバーが、Copilot CLI セッションと Claude エージェントセッションでも透過的に利用できるようになった
- `mcp.json` に定義されたサーバーや拡張機能として追加した MCP サーバーが、別途設定なしにセッション間で共有される
- チームで共有している MCP ツール（DB アクセス・Terraform Cloud・Jira・Confluenceなど）を一度設定すれば、すべてのエージェントタイプで再利用可能
- CLI でのバッチ処理（夜間の大規模リファクタリング等）でも MCP ツールを活用でき、自動化の範囲が拡大する

#### 前提条件

- **VS Code バージョン**: v1.113 以上
- **GitHub Copilot プラン**: Business / Enterprise（Copilot CLI は追加設定が必要）
- **必要な設定**: GitHub CLI がインストール済みで、`gh extension install github/gh-copilot` で CLI 拡張を追加済み
- **OS**: Windows 対応

#### セットアップ手順

1. **MCP サーバーを VS Code に登録する**
   - コマンドパレット（`Ctrl+Shift+P`）→ `MCP: Add Server` を選択する
   - または `.vscode/mcp.json` に直接追加する：
     ```json
     {
       "servers": {
         "my-infra-tool": {
           "type": "http",
           "url": "https://my-mcp-server.example.com/sse"
         }
       }
     }
     ```

2. **Copilot CLI セッションを開始する**
   - VS Code のチャットビューで `@cli` タグを使い、CLI エージェントセッションを開始する

3. **MCP ツールが引き継がれることを確認する**
   - CLI セッションで「利用可能なツールを一覧して」などと入力し、VS Code 側で設定した MCP ツールが表示されることを確認する

> 💡 **ヒント**: `Chat: Open Customizations` でどの MCP サーバーが現在有効かを確認できます。

#### 使い方（ハンズオン）

**シナリオ**: Terraform Cloud MCP サーバーを VS Code に登録しており、夜間の大規模な IaC 変更を Copilot CLI バッチタスクで自動実行したい

**この機能がない場合**:
- CLI セッションでは MCP が使えないため、Terraform Cloud の状態確認やプラン操作がエージェントから直接できない
- MCP ツールを使った VS Code ローカルセッションとコマンドラインの手動作業を組み合わせる必要がある

**操作手順**:

1. VS Code の `mcp.json` に Terraform Cloud MCP サーバーを登録しておく
2. チャットで `@cli` セッションを開始する
3. 「Terraform Cloud の現在のワークスペース一覧を取得し、最後に実行が失敗したプランを特定してください」と入力する
4. CLI エージェントが VS Code 登録済みの Terraform Cloud MCP ツールを呼び出して情報を取得する様子を確認する

**期待される結果**:
- CLI セッションから Terraform Cloud MCP ツールが呼び出され、ワークスペース情報が取得される
- VS Code とは独立した CLI バッチで MCP 連携が機能する

#### ベストプラクティス（IT インフラ業務向け）

- MCP サーバーはワークスペースの `.vscode/mcp.json` で定義することで、チームメンバーと設定を Git 経由で共有する
- 社内システムへのアクセス権を持つ MCP サーバーは、VS Code のワークスペース信頼機能と組み合わせて信頼済み環境でのみ有効化する

#### 参考リンク

- [VS Code 1.113 Release Notes – MCP support in Copilot CLI & Claude agents](https://code.visualstudio.com/updates/v1_113#_mcp-support-in-copilot-cli-claude-agents)
- [Using MCP servers in VS Code](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)
- [Model Context Protocol (MCP)](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)

---

### 習得ガイド 2: Configurable thinking effort in model picker — 推論量を UI から直接制御

#### 機能詳細解説

- Claude Sonnet 4.6 や GPT-5.4 などの「推論モデル」は、返答生成前に内部でより深い思考プロセス（thinking/reasoning）を実行できる
- 推論量が多いほど精度が上がるが、レスポンスに時間がかかりコストも増加する
- モデルピッカーで Low / Medium / High を即座に切り替えることで、タスクの複雑さに応じたコスト・速度のトレードオフを最適化できる
- 設定は会話をまたいでモデル別に保持されるため、毎回設定し直す必要がない
- 従来の `github.copilot.chat.anthropic.thinking.effort` 設定は廃止されたため `settings.json` から削除する

#### 前提条件

- **VS Code バージョン**: v1.113 以上
- **GitHub Copilot プラン**: 推論モデルが含まれるプラン（Business / Enterprise 推奨）
- **必要な設定**: 推論対応モデル（Claude Sonnet 4.6・GPT-5.4 等）が選択可能なプランであること
- **OS**: Windows 対応

#### セットアップ手順

1. **廃止設定の削除**
   - `settings.json` に `github.copilot.chat.anthropic.thinking.effort` または `github.copilot.chat.responsesApiReasoningEffort` が残っている場合は削除する

2. **モデルピッカーで推論量を設定する**
   - チャット入力欄の上部にあるモデル選択ドロップダウンをクリックする
   - 推論対応モデル（例: Claude Sonnet 4.6）を選択すると、モデル名の隣に矢印（`>`）が表示される
   - 矢印をクリックして `Low` / `Medium` / `High` から選択する

> 💡 **ヒント**: ピッカーのラベルが「GPT-5.4 · High」のように現在の推論量を表示するので、設定済みかどうかが一目でわかります。

#### 使い方（ハンズオン）

**シナリオ**: セキュリティ設計のレビューには High thinking を使い、定型的な Terraform リソース追加には Low thinking を使って効率化する

**この機能がない場合**:
- 設定画面を開いて `github.copilot.chat.anthropic.thinking.effort` を手動変更する必要があった
- タスクごとに切り替える手間が大きく、結局デフォルト設定のまま使い続けることが多かった

**操作手順**:

1. 「この IAM ポリシー設計のセキュリティ上のリスクをすべて洗い出して」というタスクでは：
   - モデルピッカーから推論モデルを選択 → thinking effort を **High** に設定
   - 詳細な分析を送信して結果を確認する

2. 「S3 バケットリソースをもう一つ追加して」という定型タスクでは：
   - モデルピッカーで同じモデルを選択 → thinking effort を **Low** に切り替え
   - 素早いコード補完が返ることを確認する

**期待される結果**:
- High thinking 時：詳細な分析・設計上の懸念点・代替案が長文で返ってくる
- Low thinking 時：素早くコード断片が返ってくる
- モデルピッカーのラベルに現在の設定が表示される

#### ベストプラクティス（IT インフラ業務向け）

- セキュリティ設計・アーキテクチャレビューには High、コード補完・ドキュメント生成には Low を基本方針とする
- チームで使い方のガイドラインを共有し、コストが予算を超えないよう運用ルールを定める

#### 参考リンク

- [VS Code 1.113 Release Notes – Configurable thinking effort in model picker](https://code.visualstudio.com/updates/v1_113#_configurable-thinking-effort-in-model-picker)
- [Thinking and reasoning in VS Code](https://code.visualstudio.com/docs/copilot/concepts/language-models#_thinking-and-reasoning)

---

### 習得ガイド 3: Images preview for chat attachments — チャット添付画像の対話型ビューア

#### 機能詳細解説

- エージェントとの会話で添付した画像や、エージェントが生成した画像（統合ブラウザのスクリーンショットなど）を専用のビューアで拡大・操作できる
- セッション内の全画像をサムネイルナビで一覧でき、会話ターンごとにグループ化されているため「どのリクエストで生成された画像か」が一目瞭然
- ズームは最大 3x 以上まで可能。スクロールで連続ズーム、クリックでジャンプズーム
- Explorer ビューからも単独で利用でき、フォルダ内の画像をまとめてブラウズできる
- `imageCarousel.chat.enabled` と `imageCarousel.explorerContextMenu.enabled` で独立制御可能（両方デフォルト有効）

#### 前提条件

- **VS Code バージョン**: v1.113 以上
- **GitHub Copilot プラン**: 画像添付はすべてのプランで利用可能
- **必要な設定**: なし（デフォルト有効）
- **OS**: Windows 対応

#### セットアップ手順

1. **設定確認（デフォルト有効）**
   - コマンドパレット（`Ctrl+Shift+P`）→ `Preferences: Open Settings (UI)` で `imageCarousel.chat.enabled` が有効であることを確認する
   
2. **画像を添付してチャットを開始する**
   - チャット入力欄の `+`（コンテキスト追加）から画像ファイルを選択して添付する
   - またはエクスプローラービューから画像ファイルをチャット欄にドラッグ＆ドロップする

> 💡 **ヒント**: Explorer ビューで画像フォルダを右クリック → `Open Images in Carousel` でフォルダ内全画像を一覧できます。

#### 使い方（ハンズオン）

**シナリオ**: 既存のシステム構成図（PNG）をエージェントに渡して分析し、返ってきた修正後の構成図を対話型ビューアで拡大確認する

**この機能がない場合**:
- 添付画像が小さなサムネイルとしてのみ表示され、細部の確認のために外部ファイルを開く必要があった
- エージェントが返した複数の画像を一覧するには、個別にダウンロードするしかなかった

**操作手順**:

1. 「+（コンテキスト追加）」からシステム構成図の PNG を添付する
2. 「この構成のネットワーク経路を図示して」などの指示を入力する
3. エージェントが画像（コントの統合ブラウザスクリーンショットなど）を返したら、その画像をクリックする
4. 対話型ビューアが開くので、サムネイルで前後の画像を比較したり、細部をピンチ/スクロールで拡大する

**期待される結果**:
- 画像クリックでフルスクリーン相当のビューアが表示される
- サムネイルストリップで同一会話内の全画像を横断して比較できる

#### ベストプラクティス（IT インフラ業務向け）

- インフラ構成図のレビューセッションでは、複数の設計案を同一チャットで生成してビューアで並べて比較する
- 統合ブラウザエージェントが取得したスクリーンショットをこのビューアで確認してフィードバックする

#### 参考リンク

- [VS Code 1.113 Release Notes – Images preview for chat attachments](https://code.visualstudio.com/updates/v1_113#_images-preview-for-chat-attachments)
- [Copilot Chat – Attaching context](https://code.visualstudio.com/docs/copilot/chat/copilot-chat#_chat-context)
