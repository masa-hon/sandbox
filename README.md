## 個人スマホのgoogleから「github cli csvからissueをつくる fieldも含めるには？」で検索
### 方法１．拡張機能をつかう
ステップ 1: 拡張機能をインストールする

```
gh extension install ntsk/gh-issue-bulk-create
```

ステップ 2: CSVファイルとテンプレートを用意する
① issues.csv（流し込みたいデータ）
```
title,assignee,label,component
"ログイン画面のバグ","octocat","bug","Frontend"
"APIのレスポンス遅延","hubot","performance","Backend"
```
② template.md（Issueの本文とフィールド定義）　←issueのテンプレート機能
```
---
title: "{{title}}"
assignees:
  - "{{assignee}}"
labels:
  - "{{label}}"
  - "triage"
---

### 概要
CSVから自動生成されたIssueです。

### 対象コンポーネント
- {{component}}
```

ステップ 3: コマンドを実行する
- 準備ができたら、以下のコマンドを実行するだけで、CSVの行数分のIssueが一括で作成され、指定したフィールド（担当者やラベルなど）も自動で反映されます。
```
gh issue-bulk-create --template template.md --csv issues.csv --repo オーナー名/リポジトリ名
```

### 方法２．シェルスクリプトで1行ずつループ
# 1行目がヘッダーのCSVを読み込んでループ処理
```
tail -n +2 issues.csv | while IFS=, read -r title assignee label component; do
  gh issue create \
    --title "$title" \
    --body "コンポーネント: $component" \
    --label "$label" \
    --assignee "$assignee"
done
```

### ただ、この方法はCSV内の「カンマ」や「改行」のエスケープ処理が複雑になりがちです。そのため、最初にご紹介した gh-issue-bulk-create 拡張機能を使う方法が一番安全かつ簡単でおすすめです。


### その他
- Project（新版Board）のカスタムフィールドを追加したい場合は、Issue作成後にGraphQL API（gh api graphql）を用いてプロジェクトアイテムの値を置き換えるステップが必要。
- 上記を実施したい場合、GraphQL APIを組み込んだスクリプト例をご提示します
