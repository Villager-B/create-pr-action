# auto-commit-pr-action

## 概要
このGitHub Actionは、変更されたファイルを自動でコミットし、プルリクエストを作成するためのアクションです。ワークフロー内でファイルを変更した後、このアクションを使用することで、手動でのコミット・プッシュ・PR作成作業を自動化できます。

## 主な機能
- 📝 変更ファイルの自動コミット
- 🔄 プルリクエストの自動作成
- 🏷️ ブランチ名の自動生成（`auto/{run_id}/{run_attempt}`形式）
- 🤖 GitHub Actions Botによる自動実行

## 使い方

### 基本的な使用例

```yaml
name: Auto Commit and Create PR
on:
  workflow_dispatch:

jobs:
  auto-pr:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
      
      # ファイルを変更する処理
      - name: Modify files
        run: |
          echo "Updated content" > example.txt
      
      # 自動コミットとPR作成
      - name: Create PR
        uses: shimasan0x00/auto-commit-pr-action@v1
        with:
          message: "feat: ファイルを更新しました"
```

### 入力パラメータ

| パラメータ | 必須 | 説明 |
|-----------|------|------|
| `message` | ✅ | コミットメッセージ（PRのタイトル・本文にも使用されます） |

### 出力パラメータ

| パラメータ | 説明 |
|-----------|------|
| `branch` | 作成されたプルリクエストのブランチ名 |

### 必要な権限

このアクションを使用するには、以下の権限が必要です：

```yaml
permissions:
  contents: write      # ファイルのコミット・プッシュに必要
  pull-requests: write # プルリクエストの作成に必要
```

## 動作の流れ

1. **ブランチ作成**: `auto/{run_id}/{run_attempt}` 形式で新しいブランチを作成
2. **ファイル追加**: ワークスペース内の全変更ファイルをステージング
3. **コミット**: 指定されたメッセージでコミット
4. **プッシュ**: 新しいブランチをリモートにプッシュ
5. **PR作成**: プルリクエストを作成（タイトル・本文はコミットメッセージと同じ）

## 使用例

### 設定ファイルの自動更新

```yaml
- name: Update config
  run: |
    # 設定ファイルを更新する処理
    echo "new_config_value" >> config.json
  
- name: Create PR for config update
  uses: shimasan0x00/auto-commit-pr-action@v1
  with:
    message: "chore: 設定ファイルを更新"
```

### ドキュメントの自動生成

```yaml
- name: Generate docs
  run: |
    # ドキュメント生成処理
    ./generate-docs.sh
  
- name: Create PR for docs
  uses: shimasan0x00/auto-commit-pr-action@v1
  with:
    message: "docs: ドキュメントを自動生成"
```

## 注意事項

- このアクションは、ワークフロー内でファイルが変更されている場合に使用してください
- ブランチ名は自動生成されるため、重複の心配はありません
- コミットメッセージはPRのタイトル・本文としても使用されます
- 必要な権限が設定されていない場合、アクションは失敗します