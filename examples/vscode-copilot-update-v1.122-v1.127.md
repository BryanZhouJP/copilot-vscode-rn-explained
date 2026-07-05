# VS Code × GitHub Copilot アップデートレポート: v1.122 〜 v1.127

<!-- markdownlint-disable MD024 MD032 MD028 -->

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.122 〜 v1.127
> **リリース日**: 2026年5月28日 〜 2026年7月1日
> **生成日**: 2026-07-05
> **対象読者**: クラウド・オンプレ基盤の企画・開発・運用を担当する IT 技術者（Windows + VS Code + GitHub Copilot 環境）

---

## 目次

- [新機能・変更一覧](#新機能変更一覧)
  - [AI活用提案サマリー](#ai活用提案サマリー)
- [習得ガイド（推奨レベル順 上位3件）](#習得ガイド)

---

## 新機能・変更一覧

> 10 件のアップデートを業務活用推奨レベル順に紹介します。

> 情報収集メモ: 以下の公式URLは 404 のため取得失敗（本レポートは他の公式情報源で補完）
> - [Copilot release notes](https://code.visualstudio.com/docs/copilot/copilot-release-notes)
> - [VS Code Team Blog](https://devblogs.microsoft.com/visualstudiocode/)

### AI活用提案サマリー

今回の VS Code × GitHub Copilot アップデートの中で、**クラウド・オンプレ基盤担当の IT 技術者**が特に注目すべき活用シナリオをまとめます。

| 推奨 | 機能名 | 活用シナリオ | 期待効果 |
| ---- | ------ | ----------- | -------- |
| ★★★ | Browser tools for agents（GA） | Web管理UIの手順テストや構成確認をエージェントに実行させる | 検証工数の削減、操作漏れ防止 |
| ★★★ | Session sync + /chronicle | 障害対応・変更作業の会話履歴を検索し、週次報告や再利用ナレッジ化に使う | 属人化低減、引き継ぎ効率化 |
| ★★★ | BYOK サインイン不要 + カスタムエンドポイント | 閉域/制限環境で社内LLMを VS Code Chat に接続する | ガバナンス準拠と現場生産性の両立 |
| ★★☆ | MCP 認証強化（clientId/secret、Enterprise-managed auth） | MCP サーバー接続を既存IdP方針に合わせて標準化する | 認証設計の統一、運用リスク低減 |
| ★★☆ | Session-level/Subagent コスト可視化 | エージェント作業ごとのクレジット消費を監視して予算管理する | コスト予見性の向上 |
| ★☆☆ | 組み込み Ollama provider 廃止予定 | 公式 Ollama 拡張へ段階移行計画を立てる | 将来の設定破綻回避 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. Browser tools for agents が GA（VS Code 内ブラウザで自動検証）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.127（2026年7月1日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**エージェントが VS Code 内ブラウザを直接操作し、Web画面の検証まで一気通貫で実行できるようになった。**

- 外部 MCP サーバーなしで、ページ遷移・入力・スクリーンショット・コンソール確認が可能
- プレビュー段階を経て GA 化され、既定で有効化されたため導入ハードルが低い
- 管理者はポリシーで無効化/到達ドメイン制限が可能で、企業運用に乗せやすい

🔗 **情報源**:
- [VS Code 1.127 Release Notes](https://code.visualstudio.com/updates/v1_127)
- [Browser tools for GitHub Copilot in VS Code are generally available](https://github.blog/changelog/2026-07-01-browser-tools-for-github-copilot-in-vs-code-are-generally-available)

💡 **AI 活用提案**:
- 運用ポータルの定型確認（ログイン、設定確認、画面遷移）をエージェント検証タスクとしてテンプレート化することを検討してみてください
- 変更管理時に「エージェント実行ログ + スクリーンショット」を証跡として残す運用を試してみてください

---

### 2. Session sync と /chronicle でセッション履歴を横断活用

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.123（2026年6月3日） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode / Copilot Chat |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**チャット/エージェントセッションをGitHubアカウントに同期し、/chronicle で検索・要約できるようになった。**

- 会話本文だけでなく、編集ファイル・ブランチ・参照PR/Issueまで履歴化できる
- `/chronicle` で自然言語検索、スタンドアップ生成、改善提案取得が可能
- 複数端末・複数作業場所での再開性が高まり、障害対応の再現性が上がる

📘 **用語解説**:
- **Chronicle**: セッション履歴を検索・要約する Copilot コマンド群

🔗 **情報源**:
- [VS Code 1.123 Release Notes](https://code.visualstudio.com/updates/v1_123)
- [Gain insights across your agent sessions with /chronicle](https://github.blog/changelog/2026-06-02-gain-insights-across-your-agent-sessions-with-chronicle)

💡 **AI 活用提案**:
- 障害一次対応セッションをタグ運用し、週次で `/chronicle` から再発防止メモを自動生成することを検討してみてください
- 引き継ぎ時に「関連セッション検索クエリ」をRunbookへ併記する運用を試してみてください

---

### 3. BYOK が GitHub サインインなしで利用可能に

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.122（2026年5月28日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat / MCP |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**閉域・制限環境でも、BYOKモデルを使って VS Code Chat/Tools/MCP を利用できるようになった。**

- GitHub サインイン前提を外し、ローカル/社内エンドポイントを直接利用できる
- カスタムエンドポイント provider が Stable 化し、独自推論基盤の接続が容易
- ただしインライン提案/NESは引き続き GitHub サインインが必要

🔗 **情報源**:
- [VS Code 1.122 Release Notes](https://code.visualstudio.com/updates/v1_122)
- [Add a custom endpoint model](https://code.visualstudio.com/docs/agent-customization/language-models#_add-a-custom-endpoint-model)

💡 **AI 活用提案**:
- 社内推論基盤のAPIキー運用を BYOK provider group に集約し、監査対象を一本化することを検討してみてください
- Windows端末の閉域セグメント向けに「BYOK最小設定テンプレート」を配布して初期構築時間を短縮してみてください

---

### 4. MCP 認証の実運用機能を強化（clientId/secret と Enterprise-managed auth）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.123（2026年6月3日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | MCP |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**MCP サーバー認証で事前登録 clientId/secret を使えるようになり、企業IdP連携プレビューも追加された。**

- `mcp.json` に `oauth.clientId` を指定し、秘密情報はOS秘密ストレージで管理可能
- Enterprise-managed auth（Preview）で、組織IdP経由の一元認証フローに寄せられる
- 既存のゼロトラスト/IdP標準に合わせた接続設計を段階的に進めやすい

📘 **用語解説**:
- **ID-JAG**: クロスアプリ認可のためのOAuth拡張ドラフト

🔗 **情報源**:
- [VS Code 1.123 Release Notes](https://code.visualstudio.com/updates/v1_123)
- [MCP servers in VS Code](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)

💡 **AI 活用提案**:
- MCPサーバーの認証方式を「動的登録」と「事前登録」で棚卸しし、高機密系から `clientId + secret` へ移行することを検討してみてください

---

### 5. Autopilot が既定有効化、完了判定ロジックも強化（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.124（2026年6月10日） |
| カテゴリ | 動作変更 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**Autopilot が既定で有効となり、Advanced Autopilot でタスク完了判定の質が改善された。**

- これまで手動監視しがちだった反復処理の終了判断をモデル補助で最適化
- ループ上限を持ちつつ「完了まで自走」の期待値を高める設計
- プレビュー機能のため、本番運用前に権限・監査条件の検証が必要

🔗 **情報源**:
- [VS Code 1.124 Release Notes](https://code.visualstudio.com/updates/v1_124)
- [Auto mode in Copilot Chat available for all users](https://github.blog/changelog/2026-06-17-auto-mode-in-copilot-chat-available-for-all-users)

💡 **AI 活用提案**:
- IaCの定型改修など、検証容易な作業から Autopilot 適用範囲を段階展開してみてください

---

### 6. Agent host セッションで複数チャットを並行運用

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.126（2026年6月24日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**1つのセッション内で複数チャットを持てるようになり、実装・レビュー・文書化を同時進行しやすくなった。**

- 同一作業コンテキストを共有したままチャットを分岐し、並行進行できる
- リロード後もチャット群が復元され、途中中断からの復帰が容易
- 複数観点レビューに有効だが、変更競合時の責務分離ルールは必要

🔗 **情報源**:
- [VS Code 1.126 Release Notes](https://code.visualstudio.com/updates/v1_126)
- [VS Code 1.127 Release Notes（Multi-chat sessions改善）](https://code.visualstudio.com/updates/v1_127)

💡 **AI 活用提案**:
- 「実装」「テスト」「運用手順書」の3チャット分割を標準化し、レビュー漏れを減らす運用を検討してみてください

---

### 7. Agentセッション運用性を大幅改善（グループ化、D&D、PRバナー）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.127（2026年7月1日） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**セッションの整理・移動・PR対応が Agents window 内で完結しやすくなった。**

- セッショングループ化とドラッグ&ドロップで運用中案件を整理しやすい
- CI失敗/PRコメントを入力欄バナーで即時検知し、対処フローに接続できる
- 複数案件並行時の視認性が向上し、見落とし事故を抑えられる

🔗 **情報源**:
- [VS Code 1.127 Release Notes](https://code.visualstudio.com/updates/v1_127)

💡 **AI 活用提案**:
- 運用案件を「障害」「改善」「検証」グループで分け、PRバナー起点で対応SLAを管理することを検討してみてください

---

### 8. コスト可視化を段階強化（追加予算、セッション単位、サブエージェント単位）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.122 / 1.125 / 1.126 / 1.127 |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Copilot Chat / Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**「どこでクレジットを使ったか」がUIで追跡しやすくなり、予算統制の実務適用が進んだ。**

- モデル選択時のコスト表示、追加予算消費率、セッション総コスト、subagent消費量を可視化
- 高コストタスクの特定とチューニング（モデル選択・分割実行）を回しやすい
- 監視体制がないと可視化だけで終わるため、月次レビュー運用が前提

🔗 **情報源**:
- [VS Code 1.122 Release Notes](https://code.visualstudio.com/updates/v1_122)
- [VS Code 1.125 Release Notes](https://code.visualstudio.com/updates/v1_125)
- [VS Code 1.126 Release Notes](https://code.visualstudio.com/updates/v1_126)
- [VS Code 1.127 Release Notes](https://code.visualstudio.com/updates/v1_127)

💡 **AI 活用提案**:
- 「高額セッション上位10件」を毎月抽出し、プロンプト設計・モデルルーティング改善を行うことを検討してみてください

---

### 9. Enterprise向け管理設定の配布手段を拡張（MDM + file-based）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.125 / 1.127 |
| カテゴリ | 機能強化 |
| 対象コンポーネント | その他（Enterprise管理） |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**Copilot 管理設定を MDM 配布と managed-settings.json 配布の両方で扱えるようになった。**

- 端末管理基盤がある環境では MDM 配布で統制を強化可能
- 非MDM端末でも `managed-settings.json` で最低限の統制を適用可能
- Windows では `%ProgramFiles%\GitHubCopilot\managed-settings.json` を利用

🔗 **情報源**:
- [VS Code 1.125 Release Notes](https://code.visualstudio.com/updates/v1_125)
- [VS Code 1.127 Release Notes](https://code.visualstudio.com/updates/v1_127)
- [Enterprise managed-settings.json is generally available](https://github.blog/changelog/2026-07-01-enterprise-managed-settings-json-is-generally-available)

💡 **AI 活用提案**:
- まずは plugin 許可ポリシーのみ file-based 配布で導入し、次段で MDM 一元管理へ寄せる段階設計を検討してみてください

---

### 10. 廃止予定: 組み込み Edit mode / 組み込み Ollama provider

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code 1.126 / 1.127 |
| カテゴリ | 廃止予定 |
| 対象コンポーネント | Copilot Chat / Agent Mode |
| **業務活用推奨レベル** | **★☆☆ 動向ウォッチ推奨** |

**既存運用に影響しうる廃止予告が出たため、移行計画を前倒しで準備すべき段階に入った。**

- 組み込み Edit mode は非推奨となり、Agent mode への移行が推奨された
- 組み込み Ollama provider も非推奨化され、公式拡張への切替が推奨された
- 互換運用が残る期間に、設定棚卸しと利用者ガイド更新を進める必要がある

🔗 **情報源**:
- [VS Code 1.126 Release Notes](https://code.visualstudio.com/updates/v1_126)
- [VS Code 1.127 Release Notes](https://code.visualstudio.com/updates/v1_127)

💡 **AI 活用提案**:
- 全端末の設定監査で Edit mode/Ollama built-in 利用有無を抽出し、移行対象リストを先に確定してみてください

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: Browser tools for agents（GA）

#### 機能詳細解説

- エージェントがコード生成だけでなくブラウザ操作まで担えるため、実装→検証→修正のループを VS Code 内で閉じられます
- UI確認作業や再現テストを手作業で繰り返す負荷を下げ、インフラ管理画面の変更検証を定型化できます
- 企業環境では利用許可ドメインを制限できるため、セキュリティ要件と両立しやすいです

#### 前提条件

- **VS Code バージョン**: 1.127 以上
- **GitHub Copilot プラン**: Copilot が利用可能なプラン
- **必要な設定**: `workbench.browser.enableChatTools`（組織ポリシーで制御される場合あり）
- **OS**: Windows

#### セットアップ手順

1. **ブラウザツール有効状態を確認**
   - コマンドパレットで `Preferences: Open Settings (UI)` を開く
   - `browser chat tools` を検索し、組織方針上有効か確認する

2. **エージェントチャットを開始**
   - Copilot Chat を開き、エージェントモードのセッションを開始する
   - 検証したいURLと期待動作を自然言語で指示する

3. **結果と証跡を確認**
   - スクリーンショット取得、要素操作、エラー確認の実行結果をチャットで確認する
   - 必要に応じて修正指示を追加し、再検証を回す

> 💡 **ヒント**: 操作対象が多い場合は「1ステップ1期待結果」で指示すると再現性が上がります
> - コマンドパレット: `Ctrl+Shift+P` → `Chat: Open Chat`
> - 設定 UI: ファイル → 基本設定 → 設定 → `workbench.browser.enableChatTools`

#### 使い方（ハンズオン）

**シナリオ**: 監視ポータルの設定画面で、通知先変更後に保存・再表示が正しく反映されるかを短時間で確認したい

**この機能がない場合**:
- 担当者が手作業で画面遷移・入力・確認を行い、検証手順の再現性が落ちる
- スクリーンショット証跡の取り忘れや、確認観点の漏れが起きやすい

**操作手順**:

1. チャットに「対象URL、ログイン手順、変更内容、確認ポイント」を箇条書きで渡す
2. 「変更実施後にスクリーンショットを撮り、差分を要約して」と指示する
3. 失敗時は「コンソールエラー確認→再試行→原因候補提示」まで自動実行させる

**期待される結果**:
- 操作ログと画面証跡がセットで残り、検証報告に転用しやすい
- 異常時に再現手順とエラー候補が同時に得られ、一次切り分けが速くなる

#### ベストプラクティス（IT インフラ業務向け）

- 本番相当環境では到達可能ドメインを最小化し、検証対象のみ許可する
- 指示文に「禁止操作（削除、公開、課金操作）」を明記して安全柵を作る
- 検証テンプレート（対象URL、前提、期待結果）をチーム共有し品質を揃える

#### 参考リンク

- [VS Code 1.127 Release Notes](https://code.visualstudio.com/updates/v1_127)
- [Browser tools for agents](https://code.visualstudio.com/docs/debugtest/integrated-browser#_browser-tools-for-agents)
- [Build and test web apps with browser agent tools](https://code.visualstudio.com/docs/agents/guides/browser-agent-testing-guide)

---

### 習得ガイド 2: Session sync + /chronicle

#### 機能詳細解説

- セッション履歴がGitHubアカウント側に同期されるため、端末をまたいだ再開が容易になります
- `/chronicle` で自然言語検索ができ、過去の変更意図や対応経緯を引き出せます
- 障害対応や運用改善のナレッジ化に直結し、引き継ぎコストを下げられます

#### 前提条件

- **VS Code バージョン**: 1.123 以上
- **GitHub Copilot プラン**: Copilot が利用可能なプラン
- **必要な設定**: `chat.sessionSync.enabled`
- **OS**: Windows

#### セットアップ手順

1. **Session sync を有効化**
   - 設定で `chat.sessionSync.enabled` をオンにする
   - ステータスバーの Copilot 状態で同期状態を確認する

2. **履歴を残す運用を整える**
   - セッション名に案件IDや障害番号を付ける
   - 主要な判断時に要約メモを会話内へ残す

3. **/chronicle で検索・要約**
   - `/chronicle` で「先週の障害対応」「特定ファイルの変更背景」などを問い合わせる

> 💡 **ヒント**: セッション名規約を先に統一すると検索精度が上がります
> - コマンドパレット: `Ctrl+Shift+P` → `Chat: Open Chat`
> - 設定 UI: ファイル → 基本設定 → 設定 → `chat.sessionSync.enabled`

#### 使い方（ハンズオン）

**シナリオ**: 週次運用報告で、複数案件の対応内容と成果を短時間で要約したい

**この機能がない場合**:
- 各担当者のローカル履歴頼みになり、報告作成に時間がかかる
- 過去判断の根拠が散逸し、再発時の調査速度が落ちる

**操作手順**:

1. 1週間の作業を案件別セッションで記録する
2. 週末に `/chronicle standup` 相当の要約指示を実行する
3. 生成結果を運用報告テンプレートへ貼り、補足だけ手編集する

**期待される結果**:
- 報告作成時間を短縮し、記述の漏れを抑制できる
- 次回同種案件で、過去対応の再利用が容易になる

#### ベストプラクティス（IT インフラ業務向け）

- セッション命名規約を「日付-案件ID-目的」で統一する
- 重大障害対応は節目ごとに人間レビューコメントを残す

#### 参考リンク

- [VS Code 1.123 Release Notes](https://code.visualstudio.com/updates/v1_123)
- [Session Sync](https://code.visualstudio.com/docs/agents/sessions/session-sync)
- [Chronicle](https://code.visualstudio.com/docs/agents/sessions/session-insights)

---

### 習得ガイド 3: BYOK サインイン不要運用

#### 機能詳細解説

- GitHub サインインが難しい環境でも、BYOK で Chat/Tools/MCP を継続利用できます
- カスタムエンドポイント対応で、社内推論基盤や互換APIへの接続自由度が上がります
- 一方でインライン提案は対象外のため、用途を「対話・実行支援」に寄せる設計が必要です

#### 前提条件

- **VS Code バージョン**: 1.122 以上
- **GitHub Copilot プラン**: BYOK運用方針に従う（契約形態は組織ポリシー準拠）
- **必要な設定**: 利用モデルの provider 設定、必要なら `chat.utilityModel` / `chat.utilitySmallModel`
- **OS**: Windows

#### セットアップ手順

1. **Language Models 管理画面を開く**
   - `Chat: Manage Language Models` を実行する

2. **BYOK provider を追加**
   - Azure/OpenAI/Anthropic/Ollama/Custom Endpoint などを選択
   - APIキーやエンドポイントを登録する

3. **ユーティリティモデルを設定**
   - サインインなし運用時は `chat.utilityModel` と `chat.utilitySmallModel` を BYOK 側へ割り当てる

> 💡 **ヒント**: 組織内配布用に provider JSON の雛形を先に作ると展開が速いです
> - コマンドパレット: `Ctrl+Shift+P` → `Chat: Manage Language Models`
> - 設定 UI: ファイル → 基本設定 → 設定 → `chat.utilityModel`

#### 使い方（ハンズオン）

**シナリオ**: 閉域ネットワーク内のサーバー更改手順書を、社内モデルだけで作成・改善したい

**この機能がない場合**:
- IDE内での対話支援が使えず、別ツールへ往復する必要がある
- 監査要件上、外部接続可否の確認作業が増える

**操作手順**:

1. BYOK接続後、手順書ドラフトをチャットで作成させる
2. 既存Runbookとの差分比較を指示する
3. 変更点に対するリスク確認質問を追加し、最終版を整える

**期待される結果**:
- 閉域要件を満たしたまま、文書作成とレビューの速度が向上する
- 変更理由と差分が明確になり、監査説明がしやすくなる

#### 参考リンク

- [VS Code 1.122 Release Notes](https://code.visualstudio.com/updates/v1_122)
- [Bring your own language model key](https://code.visualstudio.com/docs/agent-customization/language-models#_bring-your-own-language-model-key)
- [Add a custom endpoint model](https://code.visualstudio.com/docs/agent-customization/language-models#_add-a-custom-endpoint-model)

---
