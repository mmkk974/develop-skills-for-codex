# Prisma Security Checklist

## Secrets

- [ ] `schema.prisma` に実トークンを直書きしていない。
- [ ] `.env*` がGit追跡されていない（`.env.example` は除く）。
- [ ] CIでシークレットマスクが有効化されている。
- [ ] トークン分離（runtime / migration）が維持されている。

## Migration Safety

- [ ] 破壊的変更を検知した場合の段階移行案がある。
- [ ] `prisma migrate dev` の結果をレビューしている。
- [ ] 本番は `migrate deploy` のみに制限している。
- [ ] 緊急時を除き `db push` を本番で使わない。

## Schema Quality

- [ ] PK/UK/FK/Index が要件どおり明示されている。
- [ ] 監査列（createdAt/updatedAt等）を統一している。
- [ ] nullableの採用理由が説明可能である。

## Operational Hygiene

- [ ] `prisma/migrations` がGitで管理されている。
- [ ] PRやIssueに秘密情報を記載していない。
- [ ] ローテーション手順と失効手順が定義済みである。
