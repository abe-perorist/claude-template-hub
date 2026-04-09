# claude-template-hub

Claude Code テンプレートのキュレーションレジストリ。
`/template` スキルから参照され、プロジェクトへの適用に使われる。

## 構成

```
registry.json          # テンプレートのインデックス（スキルが参照するエントリポイント）
templates/
  automation/          # hooks・スキル・定期実行など自動化系
  quality/             # CLAUDE.md・メモリシステムなど応答品質・設定系
  team/                # チーム利用・共有設定系
  stack/               # 技術スタック別設定（Next.js・Python等）
```

## テンプレートの追加方法

1. `templates/{category}/` に内容ファイルを追加する
2. `registry.json` の `templates` 配列にエントリを追加する

### registry.json のスキーマ

```json
{
  "id": "unique-id",
  "name": "テンプレート名",
  "category": "quality | automation | team | stack",
  "description": "1〜2行の説明",
  "files": [
    {
      "action": "create | merge | append",
      "target": "適用先のパス（{project_slug}等のプレースホルダー可）",
      "source": "registry.jsonからの相対パス"
    }
  ],
  "notes": "適用後にユーザーに伝える補足（任意）"
}
```

## 将来の拡張

- GitHub raw URLをregistryのsourceとして指定し、コミュニティからのテンプレートを取得できるようにする
- Webサイト（キュレーションサイト）と連携し、APIでテンプレート一覧を取得する
