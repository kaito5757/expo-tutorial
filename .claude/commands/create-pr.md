# Custom Command: Create Pull Request

このガイドでは、GitHub CLI を使用してプロジェクトでプルリクエストを作成する方法を説明します。

## 前提条件

1. GitHub CLI をまだインストールしていない場合はインストールします：

   ```bash
   # macOS
   brew install gh

   # Windows
   winget install --id GitHub.cli

   # Linux
   # https://github.com/cli/cli/blob/trunk/docs/install_linux.md の手順に従ってください
   ```

2. GitHub で認証します：

```bash
  gh auth login
```

## 新しいプルリクエストの作成

1. まず、`.github/pull_request_template.md`のテンプレートに従って PR の説明を準備します

2. `gh pr create`コマンドを使用して新しいプルリクエストを作成します：

   ```bash
   # 基本的なコマンド構造
   gh pr create --title "✨(scope): 説明的なタイトル" --body "PRの説明" --base main --draft
   ```

   適切なフォーマットでより複雑な PR 説明を作成する場合は、正確な PR テンプレート構造で`--body-file`オプションを使用します：

   ```bash
   # 適切なテンプレート構造でPRを作成
   gh pr create --title "✨(scope): 説明的なタイトル" --body-file <(echo -e "## Issue\n\n- resolve:\n\n## Why is this change needed?\nここに説明を記入。\n\n## What would you like reviewers to focus on?\n- ポイント1\n- ポイント2\n\n## Testing Verification\nこれらの変更をどのようにテストしたか。\n\n## What was done\npr_agent:summary\n\n## Detailed Changes\npr_agent:walkthrough\n\n## Additional Notes\n追加の注記。") --base main --draft
   ```

## ベストプラクティス

1. **PR タイトルフォーマット**: 絵文字付きの conventional commit フォーマットを使用

   - タイトルの最初に適切な絵文字を必ず含める
   - 実際の絵文字文字を使用（`:sparkles:`のようなコード表現ではなく）
   - 例：
     - `✨(supabase): ステージングリモート設定を追加`
     - `🐛(auth): ログインリダイレクトの問題を修正`
     - `📝(readme): インストール手順を更新`

2. **説明テンプレート**: `.github/pull_request_template.md`の PR テンプレート構造を常に使用：

   - Issue 参照
   - 変更が必要な理由
   - レビューの重点項目
   - テスト検証
   - PR-Agent セクション（`pr_agent:summary`と`pr_agent:walkthrough`タグをそのまま保持）
   - 追加の注記

3. **テンプレートの正確性**: PR 説明がテンプレート構造に正確に従っていることを確認：

   - PR-Agent セクション（`pr_agent:summary`と`pr_agent:walkthrough`）を変更または名前変更しない
   - すべてのセクションヘッダーをテンプレートに表示されるとおりに保持
   - テンプレートにないカスタムセクションを追加しない

4. **ドラフト PR**: 作業が進行中の場合はドラフトとして開始
   - コマンドで`--draft`フラグを使用
   - 完了したら`gh pr ready`を使用してレビュー準備完了に変換

### 避けるべき一般的な間違い

1. **不正確なセクションヘッダー**: 常にテンプレートの正確なセクションヘッダーを使用
2. **PR-Agent セクションの変更**: `pr_agent:summary`と`pr_agent:walkthrough`プレースホルダーを削除または変更しない
3. **カスタムセクションの追加**: テンプレートで定義されているセクションのみを使用
4. **古いテンプレートの使用**: 常に現在の`.github/pull_request_template.md`ファイルを参照

### セクションの欠落

一部が「N/A」または「なし」とマークされている場合でも、常にすべてのテンプレートセクションを含める

## その他の便利な GitHub CLI PR コマンド

PR を管理するための追加の便利な GitHub CLI コマンド：

```bash
# 自分のオープンなプルリクエストを一覧表示
gh pr list --author "@me"

# PRステータスを確認
gh pr status

# 特定のPRを表示
gh pr view <PR番号>

# PRブランチをローカルでチェックアウト
gh pr checkout <PR番号>

# ドラフトPRをレビュー準備完了に変換
gh pr ready <PR番号>

# PRにレビュアーを追加
gh pr edit <PR番号> --add-reviewer username1,username2

# PRをマージ
gh pr merge <PR番号> --squash
```

## PR 作成用テンプレートの使用

一貫した説明で PR 作成を簡単にするために、テンプレートファイルを作成できます：

1. PR テンプレートで`pr-template.md`という名前のファイルを作成
2. PR 作成時に使用：

```bash
gh pr create --title "feat(scope): タイトル" --body-file pr-template.md --base main --draft
```

## 関連ドキュメント

- [PR テンプレート](/.claude/commands/.github/pull_request_template.md)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub CLI ドキュメント](https://cli.github.com/manual/)
