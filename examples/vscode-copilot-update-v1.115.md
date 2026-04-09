# VS Code × GitHub Copilot アップデートレポート: v1.115

> **対象**: Visual Studio Code + GitHub Copilot
> **対象バージョン**: v1.115
> **リリース日**: 2026-04-08
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
| ★★★ | `send_to_terminal` ツール | バックグラウンドターミナルに追加コマンドを送信し、ビルド→テスト→デプロイを継続的にエージェントが管理 | ターミナル多重起動の削減・コンテキスト継承 |
| ★★★ | 統合ブラウザツールの改善 | E2E テストの長時間スクリプト実行時もエージェントがタイムアウトせず完走 | Web 自動化の安定性向上 |
| ★★☆ | VS Code Agents コンパニオンアプリ | CI/CD パイプラインや夜間バッチをヘッドレスで Copilot エージェントが自律実行 | GUI なしの完全自律エージェント実行 |
| ★★☆ | Edit Mode の廃止予定（v1.125 削除） | `.github/copilot-instructions.md` への移行計画を今期中に立案・実施 | 廃止前の安全な移行 |

> **凡例**: ★★★ 今すぐ活用推奨 / ★★☆ 近い将来検討推奨 / ★☆☆ 動向ウォッチ推奨

---

### 1. `send_to_terminal` tool — バックグラウンドターミナルへのコマンド送信ツール

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.115（2026-04-08） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**エージェントが既存のバックグラウンドターミナルに追加コマンドを送信できる `send_to_terminal` ツールが追加され、実行中のプロセスとのインタラクションが可能になった。**

- これまでエージェントが使えるのは新規ターミナルを作成して実行する `run_in_terminal` のみで、既存の実行中プロセス（開発サーバー・ウォッチプロセス等）に追加入力できなかった
- `send_to_terminal` により既存バックグラウンドターミナルの ID を指定して追加コマンドを送信できる
- 例: 開発サーバーが起動したまま別のターミナルセッションにリロードコマンドを送る
- バックグラウンドターミナルを複数作成せずに済み、エージェントのワークスペース管理が効率化される

📘 **用語解説**:
- **バックグラウンドターミナル**: エージェントが `run_in_terminal`（`isBackground: true`）で起動した、非ブロッキングで実行し続けているターミナルセッション

🔗 **情報源**:
- [VS Code 1.115 Release Notes – send_to_terminal tool](https://code.visualstudio.com/updates/v1_115#_send-to-terminal-tool)
- [GitHub Copilot in VS Code – April 2026 Releases](https://github.blog/changelog/2026-04-08-github-copilot-in-visual-studio-code-april-releases)

💡 **AI 活用提案**:
- `npm run dev` で起動した開発サーバーにエージェントがホットリロードをトリガーしたり、Flask アプリの実行ターミナルに `reload` を送り返すシナリオで活用してください
- ビルド→テスト→デプロイのワークフローを単一エージェントセッションで管理する場合に、既存ターミナルセッションを再利用できます

---

### 2. Browser agent tools improvements — 統合ブラウザエージェントツールの使いやすさ向上

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.115（2026-04-08） |
| カテゴリ | 機能強化 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★★ 今すぐ活用推奨** |

**統合ブラウザのエージェントツールについて、ツールラベルの改善・長時間スクリプト実行サポート・タブ重複の削減の 3 つの改善が行われた。**

- **ツールラベルの改善**: Copilot チャットに表示されるブラウザツールの名称がより直感的になり、何を実行しているかが分かりやすくなった
- **長時間スクリプト実行サポート**: E2E テスト・ページ全体のスクレイピングなど時間のかかる JavaScript スクリプトが途中でタイムアウトしなくなった
- **タブ重複の削減**: エージェントが同じ URL を繰り返し開くたびに新規タブが作成されていた問題が修正され、既存タブを再利用するようになった
- 設定変更不要。既存の統合ブラウザエージェント設定のまま改善が反映される

🔗 **情報源**:
- [VS Code 1.115 Release Notes – Browser agent tools improvements](https://code.visualstudio.com/updates/v1_115#_browser-agent-tools-improvements)
- [Integrated browser documentation](https://code.visualstudio.com/docs/debugtest/integrated-browser)

💡 **AI 活用提案**:
- 本番環境の HTTPS ダッシュボード（Grafana・Kubernetes Dashboard 等）をエージェントが繰り返し確認するシナリオで、タブが増え続けることなく使えるようになります
- 長時間の E2E テストスクリプトをブラウザエージェントで実行してもタイムアウトしなくなったため、大規模な回帰テスト自動化に向いています

---

### 3. VS Code Agents companion app — ヘッドレスエージェント実行コンパニオンアプリ（Preview）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.115（2026-04-08） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**VS Code エージェントをヘッドレス（GUI なし）で CLI から起動できるコンパニオンアプリ（Preview）が追加された。Claude Code や Copilot CLI と同様に VS Code エージェントをターミナルまたはスクリプトから自律実行できる。**

- `code-agents` コマンド（仮称）でエージェントセッションをヘッドレス起動し、指示文を渡して自律的にタスクを実行させる
- VS Code のエージェント機能（MCP・カスタムエージェント・インストラクション等）がすべて引き継がれる
- CI/CD パイプライン（GitHub Actions・Azure DevOps 等）への組み込みが将来的に可能になる
- Preview のため今後 API・コマンド名・動作が変更になる可能性がある

📘 **用語解説**:
- **ヘッドレス実行**: GUI（ウィンドウ画面）なしでコマンドラインやスクリプトからアプリケーションを起動し、処理を自動化すること

🔗 **情報源**:
- [VS Code 1.115 Release Notes – VS Code Agents companion app](https://code.visualstudio.com/updates/v1_115#_vs-code-agents-companion-app-preview)
- [GitHub Copilot in VS Code – April 2026 Releases](https://github.blog/changelog/2026-04-08-github-copilot-in-visual-studio-code-april-releases)

💡 **AI 活用提案**:
- 将来的には「毎朝 9 時にインフラ構成の差分レビューを自律実行し、結果を Slack に通知する」ような自動化を VS Code エージェントで実現することを見据えて今から仕組みを検討してください

---

### 4. Background terminal notifications — バックグラウンドターミナル完了通知（Experimental）

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.115（2026-04-08） |
| カテゴリ | 新機能 |
| 対象コンポーネント | Agent Mode |
| **業務活用推奨レベル** | **★★☆ 近い将来検討推奨** |

**エージェントが起動したバックグラウンドターミナルの処理が完了したときに通知が届く機能（Experimental）が追加された。**

- 長時間実行されるバックグラウンドターミナル（例: ビルド・テスト・サーバー起動待ち）の完了を Copilot が自動検知して次のステップへ進める
- これまではエージェントが「ターミナルが完了したかどうか」を確認するためにポーリングが必要だった
- Experimental のため `github.copilot.chat.backgroundTerminalNotifications.enabled`（またはそれに相当する設定キー）を手動で有効化が必要
- GA 後は長時間エージェントタスクのコンテキスト管理が大幅に改善される見込み

🔗 **情報源**:
- [VS Code 1.115 Release Notes – Background terminal notifications](https://code.visualstudio.com/updates/v1_115#_background-terminal-notifications)

💡 **AI 活用提案**:
- 「ビルドが成功したらテストを実行し、テストが通ったらデプロイする」という連続タスクを、完了通知でトリガーして完全自動化する仕組みの先行評価に使ってください

---

### 5. Edit Mode の廃止予定（v1.125 削除） — インライン編集モードの廃止とマイグレーション

| 項目 | 内容 |
| ---- | ---- |
| 対象バージョン | VS Code v1.115（2026-04-08） |
| カテゴリ | 廃止予定 |
| 対象コンポーネント | Copilot Chat |
| **業務活用推奨レベル** | **★★☆ 今期中に移行対応推奨** |

**v1.110 から非推奨となっていた Copilot Edit Mode が、v1.125（2026 年 7 月頃見込み）で正式に削除される。現在 Edit Mode を使っているチームは今すぐ移行計画を立てる必要がある。**

- **Edit Mode とは**: v1.110 以前に使えた、チャットとは別のパネルでファイルを連続編集するモード（`Ctrl+Shift+I` で起動）
- 代替: **エージェントモード**（`#agent` タグ）または標準のチャット編集機能に移行する
- カスタム `keybindings.json` や `.github/copilot-instructions.md` で Edit Mode 特有の設定をしている場合は削除・更新が必要
- v1.125 削除時期の詳細は https://code.visualstudio.com/updates にて追跡する

⚠️ **マイグレーションチェックリスト**:
- [ ] `keybindings.json` に `copilot.edit.*` 系のキーバインドがないか確認する
- [ ] チームのドキュメント・社内 Wiki に「Copilot Edit モードの使い方」が書かれていたら「エージェントモード」に更新する
- [ ] VS Code インストラクション（`.github/copilot-instructions.md`）に Edit Mode 前提の指示が含まれていたら変更する

🔗 **情報源**:
- [VS Code 1.115 Release Notes – Edit Mode deprecation](https://code.visualstudio.com/updates/v1_115#_edit-mode-deprecation)
- [Copilot Agent Mode documentation](https://code.visualstudio.com/docs/copilot/chat/copilot-edits)

---

## 習得ガイド

> 業務活用推奨レベル順の上位 3 機能の詳細解説と習得手順です。

---

### 習得ガイド 1: `send_to_terminal` tool — バックグラウンドターミナルへのコマンド送信

#### 機能詳細解説

- エージェントモードでのターミナル操作には従来から `run_in_terminal` ツールがあったが、このツールは**新規ターミナルを作成してコマンドを実行**するもので、既存の起動中プロセスに追加で入力することができなかった
- `send_to_terminal` は既存バックグラウンドターミナルの ID を指定して追加の文字列・コマンドを送り込めるツール
- 開発サーバー・ウォッチプロセス・長時間実行中のインタラクティブプロセスに対して、エージェントが必要に応じて追加指示を送ることで、セッション全体を単一コンテキストで管理できる
- バックグラウンドターミナルを際限なく増殖させる問題が解消され、より少ないターミナルで複雑なワークフローを管理できる

#### 前提条件

- **VS Code バージョン**: v1.115 以上
- **GitHub Copilot プラン**: すべてのプランで利用可能（エージェントモードが使えるプラン）
- **必要な設定**: エージェントモードが有効（デフォルト有効）
- **OS**: Windows 対応

#### セットアップ手順

1. **エージェントモードを開く**
   - チャットビューを開く（`Ctrl+Alt+I`）
   - モードセレクターで「エージェント」を選択する

2. **バックグラウンドターミナルを起動させる**
   - 「npm run dev をバックグラウンドで起動して」などと入力する
   - エージェントが `run_in_terminal`（isBackground: true）でサーバーを起動する

3. **既存ターミナルに追加コマンドを送る**
   - 「起動中の dev サーバーのターミナルに reload と入力して」などと指示する
   - エージェントが `send_to_terminal` で既存ターミナルにコマンドを送ることを確認する

> 💡 **ヒント**: エージェントがどのターミナルに送信するかは会話の文脈から自動判断されますが、「ターミナル ID: xxx に送信して」と明示することもできます。

#### 使い方（ハンズオン）

**シナリオ**: React アプリの開発サーバーを起動させ、コンポーネント修正後に既存サーバーターミナルでホットリロードを確認させる

**この機能がない場合**:
- `npm run dev` でサーバーを起動した後、追加コマンドを送るたびに新しいターミナルが作成されてしまい、既存のサーバープロセスと無関係なターミナルが増え続けた
- 実行中のサーバーに入力が必要な場合は手動でターミナルを操作する必要があった

**操作手順**:

1. チャットに「npm run dev をバックグラウンドで起動して、最初の出力を確認して」と入力する
2. エージェントがサーバーを起動し、起動ログを確認する
3. 別のタスク（「Button.tsx を修正して」等）を実行する
4. 「dev サーバーのターミナルで rs（restart）を送信して起動状況を確認して」と入力する
5. エージェントが `send_to_terminal` で既存サーバーターミナルに `rs` を送り、新しいターミナルを作らずに済むことを確認する

**期待される結果**:
- 既存の `npm run dev` ターミナルに `rs` が送信される
- 新しいターミナルセッションが作成されない
- エージェントがターミナル出力を確認して「再起動が完了しました」と報告する

#### ベストプラクティス（IT インフラ業務向け）

- **ビルドパイプライン自動化**: `./gradlew build` バックグラウンドターミナルが起動している状態で、テスト結果に応じて追加の Gradle サブコマンドをエージェントに送信させる
- **インタラクティブプロセス**: Python の REPL や Terraform の対話的プロビジョニングなど、入力を受け付けているプロセスとエージェントを組み合わせる（ただし入力内容を事前に確認してから承認する）

#### 参考リンク

- [VS Code 1.115 Release Notes – send_to_terminal tool](https://code.visualstudio.com/updates/v1_115#_send-to-terminal-tool)
- [Copilot Agent Mode – Terminal tools](https://code.visualstudio.com/docs/copilot/agentmode/agent-mode)
- [Agent Mode overview](https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode)

---

### 習得ガイド 2: Browser agent tools improvements — 統合ブラウザエージェントの長時間実行対応

#### 機能詳細解説

- **ツールラベルの改善**: エージェントがブラウザを操作する際にチャットビューに表示される「ツール呼び出し名」が人間が読みやすい形に改善され、「何をしているのか」が追えるようになった（例: `browser_navigate` → `Navigate to https://example.com`）
- **長時間スクリプト実行サポート**: ブラウザエージェントが使う JavaScript 実行ツール（`browser_execute_script`）が長時間処理に対応し、E2E テストスイートや全ページスクレイピングが完走できる
- **タブ重複の削減**: エージェントが以前訪問した URL を再度開こうとする際、新しいタブを作る代わりに既存タブに切り替えるようになり、統合ブラウザ内のタブが際限なく増える問題が解消された
- これらの改善は設定変更なしに自動適用される

#### 前提条件

- **VS Code バージョン**: v1.115 以上
- **GitHub Copilot プラン**: エージェントモードが使えるプラン
- **必要な設定**: 統合ブラウザが有効（`github.copilot.chat.browser.enable` が有効、またはデフォルト設定のまま）
- **OS**: Windows 対応

#### セットアップ手順

1. **統合ブラウザエージェントで作業を開始する**
   - エージェントモードでチャットに「統合ブラウザで localhost:3000 を開いてフォーム送信をテストして」などと入力する
   - エージェントが統合ブラウザを起動してページを開く

2. **長時間スクリプトの動作確認**
   - 「全ページの E2E テストを実行して結果をまとめて」など、時間のかかるテストを実行させる
   - v1.115 以降は途中でタイムアウトせず完走することを確認する

3. **タブ管理の確認**
   - 同じ URL に複数回アクセスするタスク（ダッシュボードのポーリング確認等）を実行させる
   - 同じ URL のタブが重複して開かれないことを確認する

#### 使い方（ハンズオン）

**シナリオ**: Grafana ダッシュボード（`http://localhost:3000`）を統合ブラウザで監視し、エージェントが 5 回ポーリングしてアラート状態の変化をレポートする

**この機能がない場合（v1.114 以前）**:
- 5 回のポーリングで 5 つの新しいタブが作成され、統合ブラウザが混雑
- 長時間の JavaScript 待機がタイムアウトして途中で失敗することがあった

**操作手順**:

1. チャットに「統合ブラウザで localhost:3000/dashboard を開いて、30 秒ごとに 5 回ページを確認し、アラートパネルの状態変化をまとめてレポートして」と入力する
2. エージェントが統合ブラウザで URL を開く
3. 2 回目以降のポーリングで新規タブが作成されず**既存タブがリロード**されることを確認する
4. 長時間待機が発生しても途中でタイムアウトしないことを確認する
5. 5 回分の結果をまとめたレポートが返ってくることを確認する

**期待される結果**:
- 5 回のポーリングで 1 タブのみ使用（タブの重複なし）
- 完全なレポートが途中のエラーなしに返ってくる
- チャットビューのツールラベルが明確で、何をしているかが追いやすい

#### ベストプラクティス（IT インフラ業務向け）

- **監視ダッシュボードの定期確認**: 統合ブラウザエージェントを使って社内 Kubernetes Dashboard・Grafana・Jenkins の状態を定期的にチェックするワークフローを組む。タブ重複が解消されたため長期間の確認サイクルでも使いやすくなった
- **ポート番号を明示的に指定**: エージェントに渡す URL には `localhost:PORT/path` の形式でポートを明示する。複数点へのアクセスが必要な場合は最初に URL 一覧を渡すと効率的

#### 参考リンク

- [VS Code 1.115 Release Notes – Browser agent tools improvements](https://code.visualstudio.com/updates/v1_115#_browser-agent-tools-improvements)
- [Integrated browser – VS Code docs](https://code.visualstudio.com/docs/debugtest/integrated-browser)
- [Debug with the integrated browser (Agent Mode)](https://code.visualstudio.com/docs/copilot/agentmode/agent-mode#_integrated-browser)

---

### 習得ガイド 3: VS Code Agents companion app — ヘッドレスエージェント実行（Preview）

#### 機能詳細解説

- VS Code エージェントはこれまで VS Code の GUI（チャットビュー） から実行するものだったが、v1.115 の VS Code Agents コンパニオンアプリにより CLI から起動できるようになった
- Claude Code（Anthropic の CLI エージェント）や Copilot CLI と同様のインターフェース。GUI を開かなくても VS Code のエージェント設定（MCP・カスタムエージェント・インストラクション・プラグイン等）をそのまま引き継いで動作する
- CI/CD パイプライン（GitHub Actions・Azure DevOps 等）や夜間バッチスクリプトへの埋め込みが将来的に可能になる
- Preview 段階のため、コマンド名・API・動作は今後変わる可能性がある

#### 前提条件

- **VS Code バージョン**: v1.115 以上
- **GitHub Copilot プラン**: Business / Enterprise 推奨
- **必要な設定**: VS Code Agents コンパニオンアプリのダウンロード・インストール（詳細は VS Code 1.115 リリースノートおよびドキュメントを参照）
- **OS**: Windows（Preview では一部プラットフォーム制限あり）

#### セットアップ手順

1. **コンパニオンアプリをインストールする**
   - VS Code 1.115 のリリースノート（[https://code.visualstudio.com/updates/v1_115](https://code.visualstudio.com/updates/v1_115)）の記載に従い、コンパニオンアプリを取得・インストールする

2. **VS Code の設定が引き継がれることを確認する**
   - VS Code で設定済みの MCP サーバーや `.github/copilot-instructions.md` がヘッドレスセッションでも参照されることを確認する

3. **シンプルなタスクをヘッドレスで実行する**
   - ターミナルからコンパニオンアプリ CLI を起動して、「このディレクトリの README を確認して」などの単純なタスクを実行する（予習）
   - 正常に動作したら、より複雑なタスク（コードレビュー・ファイル生成など）を試す

> ⚠️ **Preview 利用時の注意**: Preview 段階では仕様変更が頻繁に起こる可能性があります。本番 CI/CD パイプラインへの組み込みは GA 後まで待つことを推奨します。

#### 使い方（ハンズオン）

**シナリオ**: GitHub Actions ワークフローから VS Code Agents コンパニオンアプリを呼び出し、PR のコードレビューを自動実行させる（将来的なシナリオとして先行評価）

**この機能がない場合**:
- Copilot のエージェント機能を CI/CD に組み込むには Copilot CLI のみが選択肢だった
- VS Code 専用のカスタムエージェント・MCP 設定を CLI で再利用する方法がなかった

**想定する操作手順（Preview 評価用）**:

1. `code-agents` コマンド（または実際のコマンド名）でコンパニオンアプリを起動する
2. `--prompt "このリポジトリのセキュリティ上の問題点を調査して SARIF 形式でレポートして"` などの引数でタスクを指定する
3. VS Code に登録済みの MCP サーバー（例: Terraform Cloud・GitHub）がヘッドレスで呼び出されることを確認する
4. 結果がファイルまたは stdout に出力されることを確認する

**期待される結果（Preview 評価時）**:
- GUI なしでエージェントが VS Code 設定を引き継いで動作する
- CI/CD パイプラインからの自動実行が技術的に実現できることを確認できる

#### ベストプラクティス（IT インフラ業務向け）

- Preview 段階では**本番ワークフローへの組み込みを避け**、専用の検証テナント・ワークスペースで動作確認する
- GA になったタイミングで「週次 IaC コードレビュー」「PR マージ前自動セキュリティ分析」など定型的な重いタスクから順に組み込むロードマップを今から設計しておく

#### 参考リンク

- [VS Code 1.115 Release Notes – VS Code Agents companion app](https://code.visualstudio.com/updates/v1_115#_vs-code-agents-companion-app-preview)
- [GitHub Copilot in VS Code – April 2026 Releases](https://github.blog/changelog/2026-04-08-github-copilot-in-visual-studio-code-april-releases)
- [Copilot Agent Mode overview](https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode)
