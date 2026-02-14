---
name: turso-secure-bootstrap
description: Turso DBの安全な初期導入と運用を標準化するスキル。Use when creating Turso databases/branches/tokens, configuring env vars for Next.js or other web apps, and enforcing secret-safe operations in local and CI/CD environments.
---

# Turso Secure Bootstrap

Tursoを新規導入するときは、この手順を厳守する。

## 1. セキュリティ原則を固定する

- 最小権限を徹底する。
- 実行用トークンとマイグレーション用トークンを分離する。
- トークンは秘密情報ストアにのみ保存する。
- `.env*` は `.env.example` を除き Git 追跡しない。
- トークン全文をログ、画面共有、チャットに出力しない。
- 漏洩疑い時は即時失効・再発行する。

## 2. 事前確認

- Turso CLI を利用可能にする。
- `turso auth whoami` で認証状態を確認する。
- `.gitignore` に `.env*` を入れる（`.env.example` は除外対象外）。

## 3. DBとブランチを作成する

- `turso db create <db_name>` でDBを作成する。
- 必要に応じて `dev` / `staging` / `prod` ブランチを作成する。
- 記録するのはDB名、リージョン、ブランチ名などの非秘密情報のみとする。

## 4. トークンを安全に発行する

- まず `turso db tokens --help` で現行CLIのオプションを確認する。
- ランタイム用トークンを最小権限で発行する。
- スキーマ変更用トークンを別で発行する。
- 管理者相当トークンをアプリ実行に使わない。

## 5. 環境変数を分離管理する

- ローカルは `.env.local`、CIはシークレットマネージャーを使う。
- 変数名を以下に統一する。
  - `TURSO_DATABASE_URL`
  - `TURSO_AUTH_TOKEN`
  - `TURSO_MIGRATION_DATABASE_URL`
  - `TURSO_MIGRATION_AUTH_TOKEN`
- `.env.local` の権限を `chmod 600 .env.local` で制限する。
- `.env.example` にはダミー値のみを書く。

## 6. 権限境界を検証する

- ランタイム資格情報で読取クエリを実行する。
- マイグレーション資格情報でのみDDL系操作を実行する。
- ランタイム資格情報ではDDLが失敗することを確認する。

## 7. 簡単なコーディング例

```bash
# DB作成
turso db create myapp-prod

# URL取得（トークンは出力しない運用にする）
turso db show myapp-prod --url
```

```dotenv
# .env.local (実値はシークレット管理下で設定する)
TURSO_DATABASE_URL="libsql://myapp-prod-xxx.turso.io"
TURSO_AUTH_TOKEN="<runtime-token>"
TURSO_MIGRATION_DATABASE_URL="libsql://myapp-prod-xxx.turso.io"
TURSO_MIGRATION_AUTH_TOKEN="<migration-token>"
```

## 8. ローテーションと失効を運用化する

- 定期ローテーション（例: 90日）を運用する。
- ローテーション完了後に旧トークンを失効する。
- 漏洩疑い時は即時失効する。
- 監査ログにはトークン全文を残さない。

## 9. 完了前チェック

- `references/security-checklist.md` を全項目確認する。
- 1項目でも未達なら導入完了にしない。
