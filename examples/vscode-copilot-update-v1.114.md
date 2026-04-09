# VS Code × GitHub Copilot アップデートレポート: v1.114

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.114
> **リリース日**: 2026-04-01
> **生成日**: 2026-04-10
> **対象読者**: クラウド・オンプレ基盤の企画・開発・運用を担当する IT 技術者（Windows + VS Code + GitHub Copilot 環境）

---

## 目次

- [新機能・変更一覧](#新機能変更一覧)
  - [AI活用提案サマリー](#ai活用提案サマリー)
- [習得ガイド（推奨レベル順 上位3件）](#習得ガイド)

---

## 新機能・変更一覧

> 5 件のアップデートを業務活用推奨レベル順に紹介します。

### AI活用提案サマリー

今回の VS Code × GitHub Copilot アップデートの中で、**クラウド・オンプレ基盤担当の IT 技術者**が特に注目すべき活用シナリオをまとめます。

| 推奨 | 機能名 | 活用シナリオ | 期待効果 |
| ---- | ------ | ----------- | -------- |
| ★★★ | Workspace search simplification | `#codebase` の変更点を把握してインフラ IaC リポジトリの検索品質を確認・再インデックス | コード検索体験の統一・安定化 |
| ★★★ | Group policy: disable Claude agent | 組織ポリシーで Claude エージェントを無効化し、承認済みモデルのみ利用可能に | エンタープライズコンプライアンスの確保 |
| ★★★ | Copy final response | エージェントの最終回答（コード部分）を素早くドキュメントや Jira に転記 | ポストプロセス効率向上 |
| ★★☆ | Troubleshoot previous chat sessions | 昨日のデプロイ指示エージェントセッションを今日 `/troubleshoot` で事後分析 | 問題再現・調査コスト削減 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. Workspace search simplification — `#codebase` の刷新と応答インデックスの統一

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.114（2026-04-01） |
| カテゴリ | 動作変更 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**`#codebase` コンテキスト変数が「純粋なセマンティック検索」のみに刷新され、ローカルインデックスとリモートインデックスの使い分けが廃止された。エージェントモードでは必要に応じて `#codebase` が自動的に利用される。**

- これまではローカルインデックス（キーワードベース）とリモートセマンティックインデックスの 2 種類があり、状態によって動作が異なっていた
- 今後は常にセマンティック検索のみ。非セマンティックインデックスのリポジトリでは再インデックスが必要
- ワークスペースインデックスのステータスアイコンがチャットビューに常時表示され、再インデックス中・完了を視覚的に確認できる
- 大規模リポジトリでは初回の再インデックスに時間がかかることがある
- VS Code のリモートワークスペース（Codespaces・リモートSSH）でも対応

🔗 **情報源**:
- [VS Code 1.114 Release Notes – Workspace search simplification](https://code.visualstudio.com/updates/v1_114#_workspace-search-simplification)
- [Codebase search with Copilot](https://code.visualstudio.com/docs/copilot/chat/workspace-context)

⚠️ **マイグレーション注意**:
- 以前のローカルキーワードインデックスのみが構築されていたリポジトリは、`#codebase` を使うと再インデックスをトリガーする
- チャットビューのインデックス状態アイコンが「完了」になるまで待ってから使用する

💡 **AI 活用提案**:
- 大規模な IaC（Terraform・Bicep）リポジトリで `#codebase` を使った横断的な変数・リソース名検索の精度が向上します
- インデックス完了後に「このリポジトリの全セキュリティグループ定義を教えて」などのクエリで効果を確認してください

---

### 2. Group policy to disable Claude agent — IT 管理者向け Claude エージェント無効化ポリシー

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.114（2026-04-01） |
| カテゴリ | 新機能 |
| 対象コンポーネント | その他（Enterprise 管理） |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**Windows グループポリシーまたは MDM により、組織全体で Claude エージェントを無効化できるポリシーキーが追加された。**

- ポリシーキー: `Claude3PIntegration`（ブール値）
- グループポリシー、MDM（Intune 等）、`settings.json` の `github.copilot.chat.claudeAgent.enabled` のいずれかで制御可能
- 無効化した場合、ユーザーは Claude エージェントセッションを開始できなくなる
- 対象: VS Code で利用可能な Claude エージェント（Anthropic Claude 系モデルを使ったサードパーティ統合エージェント）

📘 **用語解説**:
- **Claude エージェント（VS Code）**: Anthropic Claude モデルとの統合により、VS Code でネイティブに Claude セッションを開ける機能。GitHub Copilot と並列して利用可能

🔗 **情報源**:
- [VS Code 1.114 Release Notes – Group policy to disable Claude agent](https://code.visualstudio.com/updates/v1_114#_group-policy-to-disable-claude-agent)
- [VS Code Enterprise Management – Group Policies](https://code.visualstudio.com/docs/editor/enterprise)

💡 **AI 活用提案**:
- 「組織が承認したモデル（例: GitHub Copilot 経由の GPT・Claude）のみを使用する」ポリシーを策定している場合、このグループポリシーを即座に適用することを検討してください
- Intune で VS Code ポリシーを展開して、全社員の環境に一括適用できます

---

### 3. Copy final response in chat — エージェントの最終回答のみ抽出コピー

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.114（2026-04-01） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**チャットビューのコンテキストメニュー（右クリック）から「Copy Final Response」を使うと、ツール呼び出しや中間思考を除いた最後の Markdown 回答のみをクリップボードにコピーできる。**

- エージェントモードで複数のツール呼び出しが発生した後の「最終的な回答部分」のみを効率よく抽出できる
- 既存の「Copy」コマンドとは異なり、ツール実行ログ・ファイル diff 等は含まれない
- Markdown 形式のままコピーされるため、ドキュメント（Confluence・Notion 等）への貼り付けに向いている

🔗 **情報源**:
- [VS Code 1.114 Release Notes – Copy final response in chat](https://code.visualstudio.com/updates/v1_114#_copy-final-response-in-chat)

💡 **AI 活用提案**:
- エージェントによるコードレビュー結果・設計提案を Jira チケットや Confluence ページに転記する際、ツール実行ログが混在しないため整形コストが大幅に下がります
- 週次のコードレビュー要約をエージェントに作成させ、この機能で転記するワークフローを整備してください

---

### 4. Preview videos in the image carousel — 画像カルーセルでのビデオプレビュー対応

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.114（2026-04-01） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**v1.113 で導入された画像カルーセルビューアが動画ファイルにも対応し、エージェントが添付・生成した画像・動画を同一インターフェースでプレビューできるようになった。**

- `.mp4`・`.webm` などの動画ファイルをカルーセルで直接再生可能
- v1.113 の画像ビューアと同一の UI（サムネイルナビ・ズーム等）を継承
- 統合ブラウザエージェントが記録したデモ動画のレビューに特に有効

🔗 **情報源**:
- [VS Code 1.114 Release Notes – Preview videos in the image carousel](https://code.visualstudio.com/updates/v1_114#_preview-videos-in-the-image-carousel)

💡 **AI 活用提案**:
- Web デバッグエージェントが再現録画した UI バグ動画を VS Code 内で直接確認でき、ツールの切り替えコストが削減されます

---

### 5. Troubleshoot previous chat sessions — 過去セッションの `/troubleshoot` 診断（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.114（2026-04-01） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**`/troubleshoot` コマンドが**過去のセッション**にも対応し、過去に実行したエージェントセッションのデバッグログを遡って分析できるようになった（Preview）。**

- これまで `/troubleshoot` は現在のアクティブセッションのみが対象だった
- セッション一覧から過去セッションを選択して事後分析が可能
- 失敗したバッチ処理や予期しない動作をしたエージェントタスクの原因調査に活用できる
- Preview 機能であるため今後変更の可能性あり

🔗 **情報源**:
- [VS Code 1.114 Release Notes – Troubleshoot previous chat sessions](https://code.visualstudio.com/updates/v1_114#_troubleshoot-previous-chat-sessions)
- [Debugging Copilot Chat sessions](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view)

💡 **AI 活用提案**:
- 夜間バッチで失敗したエージェントタスクを翌朝 `/troubleshoot` で事後分析するワークフローを設計してください
- セッションログをエクスポートして問題報告に添付する運用と組み合わせると効果的です

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: Workspace search simplification — `#codebase` の刷新と応答インデックスの統一

#### 機能詳細解説

- `#codebase` はこれまで「ローカルキーワードインデックス」と「リモートセマンティックインデックス」の 2 種類が共存しており、ワークスペースの状態によって動作が変わるという混乱があった
- v1.114 からは**セマンティック検索のみ**に統一。ローカルキーワード検索への自動フォールバックは廃止
- リポジトリのインデックスが「非セマンティック形式」しかない場合、`#codebase` を使った際に自動的にセマンティックインデックスを構築しにいく
- エージェントモードでは Copilot が「コードベース全体を検索する必要がある」と判断した際に、明示的な `#codebase` なしで自動的にツールとして呼び出すようになった
- インデックス状態アイコンはチャットビュー下部に常時表示

#### 前提条件

- **VS Code バージョン**: v1.114 以上
- **GitHub Copilot プラン**: すべてのプランで機能するが、セマンティックインデックスはリポジトリサイズ制限あり（Business/Enterprise は上限拡大）
- **必要な設定**: なし（自動で移行されるが、再インデックス完了を確認する必要がある）
- **OS**: Windows 対応

#### セットアップ手順（既存リポジトリの移行）

1. **インデックス状態を確認する**
   - VS Code でリポジトリを開く
   - チャットビュー下部のインデックスアイコンを確認する（「インデックスなし」「構築中」「完了」などが表示される）

2. **セマンティックインデックスを構築する**
   - チャットビューのインデックスアイコンをクリック → `Build semantic index` を選択する
   - または `#codebase` を使った検索クエリを送信するとインデックス構築が自動トリガーされる

3. **インデックス完了後に動作確認する**
   - 「このリポジトリで使われているすべての環境変数を一覧して」などのクエリを `#codebase` つきで送信して結果を確認する

> 💡 **ヒント**: 大規模リポジトリ（数千ファイル以上）では初回インデックスに数分かかることがあります。インデックス中にクエリを送ると精度が下がるため、完了を待つことを推奨します。

#### 使い方（ハンズオン）

**シナリオ**: 大規模な Terraform リポジトリで「`storage_account_name` というリソース属性が定義されている場所をすべて見つける」

**この機能がない場合**:
- キーワードインデックスとセマンティックインデックスで検索結果が異なり、どちらが正しいか判断しにくかった
- インデックス状態が見えず、何を実行しているか不明だった

**操作手順**:

1. チャットビュー下部のインデックスアイコンが「完了」になっていることを確認する
2. チャット入力に `#codebase storage_account_name が定義されているすべての場所を教えて。変数の定義元・参照元をリストアップして` と入力する
3. エージェントがセマンティック検索を実行し、定義ファイルと参照先を一覧で返すことを確認する

**期待される結果**:
- キーワード完全一致ではなく、意味的に関連するリソース定義・変数参照が広く検索される
- 結果に `[ファイル名:行番号]` のリンクが付き、クリックで即時ジャンプできる

#### ベストプラクティス（IT インフラ業務向け）

- **インデックスを定期的に更新する**: 大幅なリファクタリング後はチャットビューのアイコンメニューから手動で再インデックスを実行する
- **エージェントモードを活用する**: エージェントモードでは `#codebase` を明示しなくても自動的に使われるため、インフラ全体の横断分析タスクに適している

#### 参考リンク

- [VS Code 1.114 Release Notes – Workspace search simplification](https://code.visualstudio.com/updates/v1_114#_workspace-search-simplification)
- [Codebase search with Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/workspace-context)
- [GitHub Copilot workspace index documentation](https://code.visualstudio.com/docs/copilot/chat/workspace-context#_workspace-index)

---

### 習得ガイド 2: Group policy to disable Claude agent — Claude エージェント無効化ポリシーの適用

#### 機能詳細解説

- VS Code 1.114 から `Claude3PIntegration` という新しい管理ポリシーキーが追加された
- このポリシーを `false`（無効）に設定すると `github.copilot.chat.claudeAgent.enabled` が強制的に無効化され、ユーザーは Claude エージェントセッションを開始できなくなる
- 設定可能な場所: Windows グループポリシー（GPO）・MDM（Microsoft Intune 等）・`settings.json` の `github.copilot.chat.claudeAgent.enabled`
- 適用: 組織内の VS Code インスタンス全体に一括適用可能（GPO/MDM 経由の場合）
- ポリシー適用後、ユーザーが設定から再有効化しようとしてもロックされる

#### 前提条件

- **VS Code バージョン**: v1.114 以上
- **必要な権限**: IT 管理者権限（グループポリシーまたは MDM での設定）または VS Code 設定の管理権限
- **OS**: Windows（グループ ポリシー・Intune 経由）

#### セットアップ手順（Intune（MDM）での適用）

1. **Intune 管理センターでポリシープロファイルを作成する**
   - Microsoft Intune 管理センター（`endpoint.microsoft.com`）を開く
   - `デバイス` → `構成` → `+作成` を選択する
   - プラットフォーム: Windows 10 およびそれ以降、プロファイルタイプ: カスタム OMA-URI

2. **OMA-URI 設定を追加する**
   - 名前: `VS Code - Disable Claude Agent`
   - OMA-URI: `./Device/Vendor/MSFT/Policy/Config/.../<VS Code Policy Path>` ※ VS Code の MDM ポリシーパスを参照
   - データ型: 整数、値: `0`（無効）

> 💡 **代替手段（settings.json での個別設定）**:
> ユーザーの `settings.json` に以下を追加する（管理者による展開が必要）:
> ```json
> {
>   "github.copilot.chat.claudeAgent.enabled": false
> }
> ```
> または `// @lockdown` 機能（VS Code Enterprise）でユーザーによる変更を防ぐ

3. **ポリシーが適用されていることを確認する**
   - 対象デバイスで VS Code を開く
   - チャットビューで Claude エージェントセッションのオプションが表示されないことを確認する

#### 使い方（ハンズオン）

**シナリオ**: 金融業界向けコンプライアンスポリシーにより、社内の開発者が使用できる AI モデルを GitHub Copilot 経由のモデルのみに制限したい

**この機能がない場合**:
- 個別のユーザーが VS Code 設定から Claude エージェントを有効化でき、IT 管理者が制御できなかった
- モデル利用状況の統制がセルフサービスに依存していた

**操作手順**:

1. 上記 Intune 設定または GPO でポリシーを展開する
2. 開発者デバイスでポリシーが適用されるのを待つ（Intune ポリシー同期タイムアウトまで）
3. 開発者の VS Code で Claude エージェントが非表示になっていることを確認する
4. GitHub Copilot 経由のモデル（GPT・Claude を含む）は引き続き利用可能であることを確認する（Copilot プラン経由の Claude と Claude エージェントは別機能）

**期待される結果**:
- Claude エージェントセクションがチャットビューから消える
- `github.copilot.chat.claudeAgent.enabled` を手動で変更しようとしてもエラーになる

#### ベストプラクティス（IT インフラ業務向け）

- ポリシー展開前に影響範囲（Claude エージェントを業務利用している開発者・チーム）を確認し、関係者に事前周知する
- GitHub Copilot 経由の Claude（モデルピッカーで選択可能な Anthropic Claude）と VS Code 向け Claude エージェントは**別機能**であることをユーザーに説明しておく

#### 参考リンク

- [VS Code 1.114 Release Notes – Group policy to disable Claude agent](https://code.visualstudio.com/updates/v1_114#_group-policy-to-disable-claude-agent)
- [VS Code Enterprise Group Policy Management](https://code.visualstudio.com/docs/editor/enterprise#_group-policy)
- [Microsoft Intune – VS Code ポリシー管理](https://code.visualstudio.com/docs/editor/enterprise)

---

### 習得ガイド 3: Copy final response in chat — エージェント最終回答のみクリーンコピー

#### 機能詳細解説

- エージェントモードでは、最終的な回答が返るまでに「ツール呼び出し（ファイル読み取り・コマンド実行等）」「中間レポート」「ファイル diff」など大量のログが挟まる
- 「Copy Final Response」は、これらすべてのツール実行ログを除いた**最後の Markdown 回答のみ**をクリップボードにコピーする
- ドキュメント・Jira・Teams 等への貼り付けに向いた、クリーンな Markdown テキストが手に入る
- 操作: チャットビューの最後の回答を右クリック → `Copy Final Response` を選択

#### 前提条件

- **VS Code バージョン**: v1.114 以上
- **GitHub Copilot プラン**: すべてのプランで利用可能
- **必要な設定**: なし
- **OS**: Windows 対応

#### セットアップ手順

1. **エージェントモードでタスクを実行する**
   - エージェントモードを有効にしてチャットでタスク（コードレビュー・設計提案など）を実行する

2. **最終回答を右クリックする**
   - チャットビューで最後の Copilot 回答テキストを右クリックする
   - コンテキストメニューから `Copy Final Response` を選択する

3. **ドキュメントに貼り付けて確認する**
   - Confluence・Notion・Word などのドキュメントに貼り付け、ツールログなしのクリーンな Markdown が得られることを確認する

> 💡 **ヒント**: 既存の「Copy」コマンドは全回答（ツールログ含む）をコピーします。「Copy Final Response」は最後の回答セクションのみです。

#### 使い方（ハンズオン）

**シナリオ**: エージェントに「このリポジトリのセキュリティ上の問題点を調査してレポートしてください」と依頼し、結果を Jira チケットに転記したい

**この機能がない場合**:
- 「Copy」でコピーすると、ツール実行ログ（ファイルを読み込んでいます…、ファイルを検索しています…）が大量に含まれる
- 転記前に手動でログ部分を削除する整形作業が必要だった

**操作手順**:

1. チャットに「このリポジトリの Python コードのセキュリティ問題を OWASP Top 10 に照らして調査してレポートして」と入力する
2. エージェントが複数のファイルを読み込み・分析するのを待つ
3. 最終的な Markdown レポートが表示されたら右クリック → `Copy Final Response` を選択する
4. Jira チケットの説明欄に貼り付ける

**期待される結果**:
- 「## セキュリティ調査レポート... 」から始まるクリーンな Markdown テキストのみがクリップボードに入る
- ツール呼び出しログ・ファイルツール出力は含まれない

#### ベストプラクティス（IT インフラ業務向け）

- 定期的なコードレビュー・セキュリティポスチャレポートを Copilot エージェントで生成し、Copy Final Response → Confluence ページに貼り付けるワークフローを標準化する
- 週次 Terraform レビューを「`#codebase` 今週変更があったリソース定義をレビューして」で実行し、結果を Copy Final Response でチームに共有する

#### 参考リンク

- [VS Code 1.114 Release Notes – Copy final response in chat](https://code.visualstudio.com/updates/v1_114#_copy-final-response-in-chat)
- [Copilot Chat overview](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)
