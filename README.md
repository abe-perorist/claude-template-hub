# Claude Code Template Hub

Claude Code の設定・スキル・hookのテンプレートを目的別に集めたキュレーションレジストリ。

`/template` スキルを使うことで、テンプレートを対話的に選択して自分のプロジェクトに適用できます。

## 特徴

- **目的別カテゴリ**: quality / automation / team / stack の4分類
- **キーワード検索**: 「通知したい」「チーム開発」など自然な言葉で探せる（スキル経由）
- **プロジェクトへの自動適用**: Claudeが現在のプロジェクト構成を読んで適切に適用
- **作者・ソース明記**: すべてのテンプレートに作者情報とインスピレーション元を記載

## セットアップ

### 1. リポジトリをクローン

```bash
git clone https://github.com/abemasanori/claude-template-hub ~/Projects/claude-template-hub
```

### 2. 設定ファイルを作成

```bash
cat > ~/.claude/template-hub.json << 'EOF'
{
  "registry_path": "~/Projects/claude-template-hub/registry.json",
  "registry_url": null
}
EOF
```

### 3. スキルをインストール

```bash
cp ~/Projects/claude-template-hub/skills/template.md ~/.claude/commands/template.md
```

### 4. 使う

Claude Codeで `/template` と入力するだけ。

## テンプレート一覧

### quality — 応答品質・設定

| テンプレート | 説明 |
|---|---|
| メモリシステム | 会話をまたいでコンテキストを保持するファイルベースメモリ |
| グローバルCLAUDE.md | 日本語優先・簡潔応答・安全ルールを含むグローバル設定 |

### automation — 自動化

| テンプレート | 説明 |
|---|---|
| 通知フック（macOS） | ツール完了時に通知音を鳴らすhook |
| 日次レポート自動生成 | Backlog + Slack + Google Calendarを集約するスキル |
| 安全フック | 破壊的操作の前にユーザー確認を挟むhook |

### team — チーム開発

| テンプレート | 説明 |
|---|---|
| チーム開発共通CLAUDE.md | ブランチ戦略・PRルール・コミット規約を含む |
| コードレビュースキル | チェックリストに基づいたPRレビュースキル |

### stack — 技術スタック別

| テンプレート | 説明 |
|---|---|
| Next.js プロジェクト用CLAUDE.md | App Router・Tailwind・TypeScript向けコーディング規約 |

## テンプレートを追加する

1. `templates/{category}/` にテンプレートファイルを作成する
2. `registry.json` の `templates` 配列にエントリを追加する

```json
{
  "id": "your-template-id",
  "name": "テンプレート名",
  "category": "quality | automation | team | stack",
  "description": "1〜2行の説明",
  "keywords": ["検索キーワード"],
  "author": {
    "name": "あなたの名前",
    "url": "https://github.com/yourname"
  },
  "source": {
    "type": "original | derived | community",
    "url": "インスピレーション元のURL（任意）",
    "inspiration": "背景の説明（任意）"
  },
  "files": [
    {
      "action": "create | merge | append",
      "target": "適用先のパス",
      "source": "registry.jsonからの相対パス"
    }
  ],
  "notes": "適用後にユーザーへ伝える補足（任意）"
}
```

3. PRを送る（`source.url` に元ネタのURLを明記してください）

## ライセンス

MIT — 各テンプレートの `author` に記載された作者が著作権を保持します。
