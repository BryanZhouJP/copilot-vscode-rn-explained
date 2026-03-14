# 公式情報源 URL リスト

このスキルが情報収集に使用する公式情報源の一覧です。

---

## VS Code リリースノート

### バージョン一覧の取得（動的）

バージョンリストはスキル実行時に以下のトップページをフェッチして動的に取得する。ファイルへの手動追記は不要。

- **トップページ**: https://code.visualstudio.com/updates/

> **URL パターン**: `https://code.visualstudio.com/updates/v{MAJOR}_{MINOR}`（例: v1.111 → `v1_111`）
> トップページから直近バージョンの一覧（バージョン番号・リリース日・タイトル）を取得し、対象バージョンの URL を組み立ててフェッチすること。

---

## GitHub Copilot 公式 Changelog

- **GitHub Changelog (Copilot tag)**: https://github.blog/changelog/label/copilot/
- **GitHub Copilot in VS Code リリースノート**: https://code.visualstudio.com/docs/copilot/copilot-release-notes

---

## GitHub Copilot ドキュメント

- **GitHub Copilot 概要（docs）**: https://docs.github.com/en/copilot
- **GitHub Copilot Chat**: https://docs.github.com/en/copilot/using-github-copilot/copilot-chat/asking-github-copilot-questions-in-your-ide
- **Copilot Edits**: https://code.visualstudio.com/docs/copilot/copilot-edits
- **Agent mode（VS Code）**: https://code.visualstudio.com/docs/copilot/agents/agent-mode
- **MCP（Model Context Protocol）**: https://code.visualstudio.com/docs/copilot/chat/mcp-servers
- **カスタム指示**: https://code.visualstudio.com/docs/copilot/customization/overview
- **Copilot 設定リファレンス**: https://code.visualstudio.com/docs/copilot/reference/copilot-settings

---

## GitHub Blog（Copilot 関連）

- **GitHub Blog - AI/Copilot カテゴリ**: https://github.blog/ai-and-ml/github-copilot/
- **GitHub Next（研究・実験的機能）**: https://githubnext.com/

---

## Microsoft Tech Community

- **VS Code Team Blog**: https://devblogs.microsoft.com/visualstudiocode/
- **GitHub Copilot Blog（Microsoft）**: https://devblogs.microsoft.com/visualstudio/tag/github-copilot/

---

## フェッチ時の注意事項

1. **VS Code リリースノートのアクセス方法**:
   - 期間に合わせて対象バージョン番号を特定してから URL をフェッチする
   - GitHub Copilot に関連するセクションのみを抽出する（例: "GitHub Copilot"、"Copilot Chat"、"AI"、"Agent" 等のセクション名）

2. **バージョンフィルタリング**:
   - ステップ 1 で確定したバージョンの URL のみをフェッチ対象とする
   - VS Code の各バージョンのリリース日は https://code.visualstudio.com/updates/ のページで確認できる

3. **優先フェッチ順**:
   1. VS Code リリースノート（最も直接的な情報源）
   2. GitHub Copilot in VS Code リリースノート
   3. GitHub Changelog (Copilot tag)
   4. GitHub Blog AI/Copilot
   5. Microsoft DevBlogs
