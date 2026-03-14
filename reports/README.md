# reports/

このディレクトリは `vscode-rn-ai-explain` スキルが生成するレポートファイルの出力先です。

## ファイル命名規則

| バージョン指定 | ファイル名形式 | 例 |
| ------------- | ------------- | -- |
| 単一バージョン | `vscode-copilot-update-v{VERSION}.md` | `vscode-copilot-update-v1.111.md` |
| 範囲指定 | `vscode-copilot-update-v{FROM}-v{TO}.md` | `vscode-copilot-update-v1.109-v1.111.md` |

## 使い方

VS Code の Copilot チャット（Agent モード）で以下のように入力してください:

```
/vscode-rn-ai-explain
```

引数を省略すると、直近 6 バージョンの一覧が表示され、対話形式でバージョンを選択できます。

バージョンを直接指定する場合:

```
/vscode-rn-ai-explain latest          ← 最新バージョン
/vscode-rn-ai-explain 1.111           ← 特定バージョン
/vscode-rn-ai-explain 1.109 1.111     ← バージョン範囲
```
