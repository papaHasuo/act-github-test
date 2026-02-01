# Act テストプロジェクト

Issueがopenされたときに自動コメントするGitHub Actionsワークフローのテスト環境です。

## 必要なツール

- [act](https://github.com/nektos/act) - GitHub Actionsをローカルで実行

## インストール

```bash
# actのインストール (Linux)
curl -s https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash
```

## 使い方

### 1. ローカルでシミュレーション実行（モック）

```bash
# issue openedイベントをシミュレート（APIは実際には呼ばれない）
act issues -e .github/workflows/issue-opened.json

# ドライラン（実際には実行せず、何が実行されるかを確認）
act issues -e .github/workflows/issue-opened.json -n

# 詳細なログを表示
act issues -e .github/workflows/issue-opened.json -v
```

### 2. 実際のGitHub APIに接続してテスト

実際のGitHubリポジトリのIssueにコメントを投稿してテストする場合:

1. **GitHub Personal Access Tokenを作成**
   - [GitHub Settings > Developer settings > Personal access tokens](https://github.com/settings/tokens)
   - `repo`スコープを付与

2. **トークンファイルを作成**
   ```bash
   echo "GITHUB_TOKEN=ghp_your_token_here" > my.token
   ```

3. **issue-opened.jsonを編集**
   - テスト対象のリポジトリに存在するissue番号を設定

4. **actを実行**
   ```bash
   act issues -e .github/workflows/issue-opened.json --secret-file my.token
   ```

### 注意事項

- デフォルトではactはAPIをモックします（実際のリクエストは送信されない）
- `--secret-file`オプションを使うと、実際のGitHub APIにリクエストが送信されます
- 実際のAPIを呼ぶ場合、対象のIssueが存在しないと404エラーになります（これは正常な動作）
- トークンファイルは`.gitignore`に追加してコミットしないよう注意してください

## ファイル構成

- [.github/workflows/issue-comment.yml](.github/workflows/issue-comment.yml) - メインのワークフロー
  - トリガー: Issueがopenされたとき
  - アクション: 定型文でコメントを投稿
- [.github/workflows/issue-opened.json](.github/workflows/issue-opened.json) - actテスト用のイベントペイロード
