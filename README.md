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

### ワークフローのテスト

```bash
# issue openedイベントをシミュレート
act issues -e .github/workflows/issue-opened.json

# ドライラン（実際には実行せず、何が実行されるかを確認）
act issues -e .github/workflows/issue-opened.json -n

# 詳細なログを表示
act issues -e .github/workflows/issue-opened.json -v
```

### 注意事項

- actで実行する場合、実際のGitHub APIへのリクエストは行われません
- `secrets.GITHUB_TOKEN`は自動的にモックされます
- より詳細なテストを行う場合は、`.secrets`ファイルにトークンを設定できます

## ワークフローの内容

[.github/workflows/issue-comment.yml](.github/workflows/issue-comment.yml)

- トリガー: Issueがopenされたとき
- アクション: 定型文でコメントを投稿
