# CLAUDE.md — Next.js プロジェクト

## スタック

- Next.js App Router
- TypeScript（strict mode）
- Tailwind CSS
- （必要に応じてDB・認証を追記）

## コーディング規約

- Server Component をデフォルトとし、インタラクティブな部分のみ `'use client'` を付ける
- `app/` 以下のファイルはルートセグメント単位で整理する
- データフェッチは Server Component 内で直接行い、クライアントへの props 受け渡しを最小化する
- Tailwind のクラスは既存のデザイントークンを優先し、任意の値（`[123px]`）は避ける
- `any` 型は禁止。型が不明な場合は `unknown` を使って絞り込む

## ファイル構成

```
app/
  (routes)/
  _components/   # このルート専用コンポーネント
components/      # アプリ全体で共通のコンポーネント
lib/             # ユーティリティ・型定義
```

## 禁止事項

- `useEffect` でデータフェッチしない（Server Component か SWR/React Query を使う）
- `pages/` ディレクトリを混在させない
- `.env.local` をコミットしない
