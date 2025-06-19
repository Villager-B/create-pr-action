# create-pr-action

## 概要
このリポジトリは、GitHub Actionsを用いて変更ファイルのコミット・プルリクエスト作成・リリース・バージョン管理を自動化するためのアクションおよびワークフローを提供します。

## 主な機能
- 変更ファイルの自動コミットとプルリクエスト作成
- セマンティックバージョニングに基づくバージョン管理
- GitHubリリースの自動作成
- テスト用ワークフローの提供

## 使い方
### プルリクエスト作成アクション
`action.yml` で定義されているアクションを利用することで、変更ファイルを自動でコミットし、プルリクエストを作成できます。

#### 必要なパーミッション
- `contents: write`
- `pull-requests: write`

#### 入力
- `message`: コミットメッセージ（必須）

#### 出力
- `branch`: 作成されたプルリクエストのブランチ名

### リリースワークフロー
`.github/workflows/release.yml` では、バージョンアップ（patch, minor, major）を選択してリリースを自動作成できます。

#### 実行例
GitHub Actionsの「workflow_dispatch」からバージョンアップレベル（patch, minor, major）を選択して実行します。

#### バージョン管理スクリプト
`.github/scripts/bump.sh` で最新タグを取得し、指定レベルでバージョンを自動更新・タグ付与・リモートへpushします。

### テストワークフロー
`.github/workflows/test.yml` では、プルリクエスト作成アクションの動作確認を自動で行います。

## ディレクトリ構成
- `action.yml`: プルリクエスト作成アクションの定義
- `.github/scripts/bump.sh`: バージョン管理用スクリプト
- `.github/workflows/release.yml`: リリース自動化ワークフロー
- `.github/workflows/test.yml`: テスト用ワークフロー

## 注意事項
- `.github/scripts/bump.sh` を利用する場合は、実行権限（chmod +x）が必要です。
- GitHub Actionsの実行には、必要なパーミッション設定を忘れずに行ってください。