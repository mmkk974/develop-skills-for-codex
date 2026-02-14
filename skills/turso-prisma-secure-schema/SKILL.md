---
name: turso-prisma-secure-schema
description: Turso導入後にPrismaで安全にテーブル定義・マイグレーション運用を行うスキル。Use when configuring Prisma datasource for Turso, defining schema.prisma models, and executing migration workflows with strict secret separation and production safety controls.
---

# Turso Prisma Secure Schema

Tursoを利用するプロジェクトで、Prismaを用いたテーブル定義とマイグレーションを安全に進める。

## 1. セキュリティ前提を固定する

- Prisma実行時の接続情報を「アプリ実行用」と「マイグレーション用」で分離する。
- 秘密情報は `.env.local` / CIシークレットに限定する。
- `.env` や `schema.prisma` に実トークンを直書きしない。
- 本番反映はレビュー済みマイグレーションのみ許可する。

## 2. Prisma設定を安全化する

- `schema.prisma` の `datasource` は `env()` を使う。
- 必要な環境変数を定義する。
  - `TURSO_DATABASE_URL`
  - `TURSO_AUTH_TOKEN`
  - `TURSO_MIGRATION_DATABASE_URL`
  - `TURSO_MIGRATION_AUTH_TOKEN`
- `DATABASE_URL` を使う場合は用途を固定し、混在させない。

## 3. モデル定義の実務ルール

- 主キー、ユニーク制約、外部キー、インデックスを明示する。
- `createdAt` / `updatedAt` などの監査列を標準化する。
- nullable設計は理由付きで採用する。
- enumや制約はアプリ要件と整合させる。

## 4. マイグレーション運用

- ローカルで `prisma migrate dev` を実行しSQL差分をレビューする。
- CIでは検証コマンドを実行する。
- 本番は承認済み手順でのみ `prisma migrate deploy` を実行する。
- `db push` は緊急時以外で本番利用しない。

## 5. 事故防止チェック

- 破壊的変更（列削除、型変更、NOT NULL化）を検出したら段階移行を設計する。
- バックアップまたはロールバック方針を事前に定義する。
- 大規模テーブル変更は負荷時間帯を避ける。

## 6. 簡単なコーディング例

```prisma
// prisma/schema.prisma
datasource db {
  provider = "sqlite"
  url      = env("TURSO_DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

```bash
# ローカルで差分作成と適用
pnpm prisma migrate dev --name create-user

# 本番反映（承認済みのみ）
pnpm prisma migrate deploy
```

## 7. 生成物と機密管理

- `prisma/migrations` はGit管理する。
- `.env*` の実値はGit管理しない。
- PR本文に接続先URLやトークンを載せない。

## 8. 完了前チェック

- `references/prisma-security-checklist.md` を全項目確認する。
- 未達項目がある状態で適用しない。
