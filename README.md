##個人スマホのgoogleから「github cli csvからissueをつくる fieldも含めるには？」で検索
### 拡張機能をつかう
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
