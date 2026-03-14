# VS Code × GitHub Copilot 新機能キャッチアップ — Agent Skill

> 対象: VS Code + GitHub Copilot (Agent モード) | 言語: 日本語レポート生成

## 概要

VS Code の公式リリースノートおよび GitHub Copilot 関連ドキュメントから、指定バージョンのアップデートを収集・分析し、**クラウド・オンプレ基盤を担当する IT 技術者**向けのマークダウンレポートを自動生成する GitHub Copilot Agent Skill です。

### 生成されるレポートの内容

- 🔍 **新機能・変更一覧** — 業務活用推奨レベル順（★★★/★★☆/★☆☆）に整理。各トピック 300 文字以内の要点解説＋公式情報源 URL
- 💡 **AI 活用提案サマリー** — IT 技術者が実務で検討できる具体的な活用シナリオ一覧（優先度付き）
- 📖 **習得ガイド** — 業務活用推奨レベル順の上位 3 機能について（★★★ → ★★☆ → ★☆☆ の優先順）、すぐ試せる詳細な手順・ベストプラクティスを提供

## 対象読者

クラウドとオンプレの基盤製品の企画・開発・運用を担当する IT 技術者。Windows マシンに VS Code がインストール済みで、GitHub Copilot との連携が設定済みであること。

## ファイル構成

```
.
├── README.md                                        # このファイル
├── reports/                                         # 生成レポートの出力先
│   ├── README.md
│   └── vscode-copilot-update-v{VERSION}.md          # スキル実行時に生成
└── .github/
    └── skills/
        └── vscode-rn-ai-explain/
            ├── SKILL.md                             # スキルのメイン手順（エントリーポイント）
            ├── assets/
            │   └── sources.md                       # 公式情報源 URL リスト
            └── references/
                └── output-format.md                 # レポート出力フォーマット仕様
```

### 各ファイルの役割

| ファイル | 役割 |
| ------- | ---- |
| `reports/` | スキルが生成するレポートファイルの出力先 |
| `SKILL.md` | スキルのエントリーポイント。情報収集〜レポート生成の 5 ステップ手順を定義 |
| `assets/sources.md` | VS Code・GitHub Copilot の公式情報源 URL リスト |
| `references/output-format.md` | レポートのマークダウン構造・テンプレート・記述ルールを定義 |

## 前提条件

- [Visual Studio Code](https://code.visualstudio.com/) 最新版
- [GitHub Copilot](https://github.com/features/copilot) サブスクリプション（Agent モード対応プラン）
- VS Code で Agent モードが有効になっていること

### 推奨モデル

| 推奨度 | モデル | 理由 |
| ------ | ------ | ---- |
| ★★★ 最推奨 | Claude Sonnet 4.5 / 4.6 | 大容量コンテキスト・高精度な tool use・日本語生成品質が高い |
| ★★☆ | GPT-4o | Agent モードでの tool use 安定性が高い |
| ★★☆ | Gemini 1.5 Pro / 2.0 Flash | 大量テキスト処理に有利 |

> 切り替え方法: Copilot チャットのモデルセレクターからスキル実行前に変更してください。

## 使い方

VS Code の Copilot チャット（Agent モード）で `/` を入力し、`vscode-rn-ai-explain` を選択します。

```
/vscode-rn-ai-explain
```

引数を省略すると、直近 6 バージョンの一覧とリリース日が表示されます。その後、対話形式で以下から選択できます:

- 番号を選択 → 特定バージョン 1 件
- `L` → 最新バージョン（自動検出）
- `R` → 範囲指定（開始・終了バージョンを追加で入力）

バージョンを直接引数で指定することもできます:

```
/vscode-rn-ai-explain latest          ← 最新バージョン
/vscode-rn-ai-explain 1.111           ← 特定バージョン
/vscode-rn-ai-explain 1.109 1.111     ← バージョン範囲指定
```

### 生成されるファイル名

| バージョン指定 | ファイル名 |
| ------------- | ---------- |
| 単一バージョン（例: 1.111） | `vscode-copilot-update-v1.111.md` |
| 範囲指定（例: 1.109〜1.111） | `vscode-copilot-update-v1.109-v1.111.md` |

ファイルは `reports/` ディレクトリに保存されます。

## 対象情報源

| 情報源 | URL |
| ------ | --- |
| VS Code リリースノート | https://code.visualstudio.com/updates/ |
| GitHub Copilot in VS Code リリースノート | https://code.visualstudio.com/docs/copilot/copilot-release-notes |
| GitHub Changelog (Copilot) | https://github.blog/changelog/label/copilot/ |
| GitHub Copilot ドキュメント | https://docs.github.com/en/copilot |
| VS Code DevBlog | https://devblogs.microsoft.com/visualstudiocode/ |

詳細な URL リストは [.github/skills/vscode-rn-ai-explain/assets/sources.md](.github/skills/vscode-rn-ai-explain/assets/sources.md) を参照してください。

## カスタマイズ

### 情報源の追加・変更

`assets/sources.md` を編集して情報源 URL を追加・変更できます。

### 対象カテゴリの追加

`SKILL.md` のステップ 2「フェッチ順序と収集方針」テーブルに対象を追加することで収集範囲を拡張できます。

### 出力フォーマットの変更

`references/output-format.md` のテンプレートを編集することで、レポートの構成・記述スタイルを変更できます。

## ライセンス

[MIT License](LICENSE)
