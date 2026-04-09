# スキル: 日次レポート自動生成

## セットアップ

このスキルを使う前に、以下の変数を自分の環境に合わせて書き換えてください：

| プレースホルダー | 内容 | 例 |
|---|---|---|
| `{BACKLOG_SPACE}` | BacklogのスペースID | `mycompany` |
| `{BACKLOG_USER_ID}` | BacklogのユーザーID（数値） | `123456` |
| `{SLACK_USER_ID}` | SlackのユーザーID | `U012AB3CD` |
| `{USERNAME}` | あなたのユーザー名 | `yamada` |
| `{REPORT_DIR}` | レポートを保存するディレクトリ名 | `daily-report` |

環境変数 `$BACKLOG_API_KEY` はシェルの設定ファイル（`~/.zshrc`等）に追加してください。

---

## 概要

毎朝、Google Calendar・Slack・Backlogから情報を収集し、当日の日次レポートをローカルのmdファイルとして生成する。
前日の未完了TODOを引き継ぎ、当日の予定・アクションアイテムを整理する。

---

## ワークフロー

### Step 1: 前日レポートから未完了TODOを抽出・検証

- `~/{REPORT_DIR}/YYYY/YYYYMMDD.md`（前日分）を読み込む
- `[ ]` のチェックボックスが未完了のTODOをすべて抽出する
- ファイルが存在しない場合は「持ち越しTODOなし」として続行する

#### Step 1-b: Slack由来TODOの解決済みチェック

- 抽出した未完了TODOのうち、`（{チャンネル名} / {投稿者名}）` の形式を含むものをSlack起源として識別する
- 各Slack起源TODOについて、Slack MCPでそのスレッドを検索・確認する
  - 自分（`{SLACK_USER_ID}`）がそのスレッドに返信済み → **持ち越しから除外**
  - 自分がそのメッセージにスタンプ（リアクション）済み → **持ち越しから除外**
  - 返信もスタンプもない → **持ち越し継続**

### Step 2: Google Calendarから当日の予定を取得

- 本日（実行日）の終日〜23:59までの予定を取得する
- 取得項目: タイトル、開始時刻、終了時刻、参加者
- イベントタイトルに ` / ` が含まれる場合は別タスクとして分割する

### Step 3: Slackから自分へのメンション・リアクションを収集

- 過去24時間以内に自分がメンションされたメッセージを取得する（上限40件）
- 自分の投稿にリアクションがついたメッセージを取得する
- アクションが必要そうな内容（依頼・質問・確認）を優先して抽出する

### Step 4: Backlogから担当タスクを収集

- 環境変数 `$BACKLOG_API_KEY` を使用してAPIを呼び出す
- スペース: `{BACKLOG_SPACE}.backlog.com` / ユーザーID: `{BACKLOG_USER_ID}`
- 担当かつステータスが未対応・処理中・処理済みのタスクを最大40件取得する
- 各タスクについて:
  - 期限が今日以前 → 「期限超過」マーク
  - 期限が明日〜3日以内 → 「期限間近」マーク
  - 最新コメントが自分以外から → 「返信必要」マーク（コメント0件は対象外）

```bash
# タスク一覧取得
curl -s "https://{BACKLOG_SPACE}.backlog.com/api/v2/issues?apiKey=$BACKLOG_API_KEY&assigneeId[]={BACKLOG_USER_ID}&statusId[]=1&statusId[]=2&statusId[]=3&count=40"
# コメント取得（最新1件）
curl -s "https://{BACKLOG_SPACE}.backlog.com/api/v2/issues/{issueId}/comments?apiKey=$BACKLOG_API_KEY&count=1&order=desc"
```

### Step 5: mdファイルを生成・保存

- 保存先: `~/{REPORT_DIR}/YYYY/YYYYMMDD.md`
- ディレクトリが存在しない場合は自動作成する
- 既存ファイルがある場合は上書きせず末尾に追記する

### Step 6: Slack DMで完了通知を送る

- 自分自身（`{SLACK_USER_ID}`）にDMで送信する:
  ```
  ✅ デイリーレポートを生成しました（{YYYY-MM-DD}）
  vscode://file/Users/{USERNAME}/{REPORT_DIR}/{YYYY}/{YYYYMMDD}.md
  ```

---

## 出力フォーマット

```markdown
# {YYYY-MM-DD} デイリーレポート

## 📅 今日の予定
{時刻} {予定タイトル}（{参加者}）

## 🔁 持ち越しTODO
- [ ] {前日から未完了のTODO}

## 📌 Slackアクション（要対応）
- [ ] {メンション・リアクション元のアクション概要} （{チャンネル名} / {投稿者名}）

## 📋 Backlogタスク（要対応）
### 🔴 期限超過
- [ ] [{issueKey}] {タスク名}（期限: {日付}）
### 🟡 期限間近（3日以内）
- [ ] [{issueKey}] {タスク名}（期限: {日付}）
### 💬 返信必要
- [ ] [{issueKey}] {タスク名} ← {最終コメント投稿者}からコメントあり

## ✏️ 今日のTODO
- [ ] {Slackや予定・Backlogから抽出した当日タスク}


---
_生成日時: {実行日時}_
```

---

## トラブルシューティング

| 症状 | 対処 |
|------|------|
| 前日ファイルが見つからない | パスのYYYY/YYYYMMDDフォーマットを確認。初回は正常動作 |
| Slackの取得件数が0件 | MCP接続・権限を確認。search:readスコープが含まれているか確認 |
| Google Calendarの予定が取得できない | MCP接続・カレンダーIDの設定を確認 |
| Backlog APIが401エラー | $BACKLOG_API_KEY が設定されているか確認（source ~/.zshrc） |
| BacklogのSSLエラー | curlを使用する（Python urllibはSSL証明書エラーが出る場合あり） |
