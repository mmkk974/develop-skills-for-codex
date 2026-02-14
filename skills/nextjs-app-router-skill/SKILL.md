---
name: nextjs-app-router-skill
description: Next.js App Router の設計・実装ルールを強制するスキル。Use when creating, reviewing, or refactoring Next.js codebases that require strict server/client boundaries, Zod validation, Suspense-first rendering, route-handler + TanStack Query CRUD, next-intl i18n, Tailwind/shadcn UI, and FSD layering without Entities.
---

# Next.js App Router Skill

このファイルのルールを実装・レビュー時に必ず適用する。

## 1. データ取得と更新の責務分離

- Client Component から直接APIを呼ばない。
- 初期データ取得は Server Component で実行する。
- 読み取り関数は `fetchHogehoge` 形式の `async` 関数で実装する。
- 取得処理は `shared/lib/apiClient` の `apiClient.get()` を使う。
- APIベースURLは環境変数から取得する。
- APIレスポンスは Zod で検証する。
- 型は Zod schema から推論して利用する。
- 更新系は Server Actions で実装する。
- Server Actions 実行後は `revalidatePath` か `revalidateTag` で無効化する。

## 2. ユーザー起点CRUD

- ユーザー操作のCRUDは Route Handlers + TanStack Query で実装する。
- インタラクションUIは Client Component に置く。
- ダイアログ等での prop バケツリレーを禁止する。
- インタラクティブ部品は必要データを自分で TanStack Query 取得する。

## 3. バリデーション戦略

- クライアントとサーバーで Zod schema を共有する。
- クライアント側は React Hook Form + Zod で検証する。
- サーバー側でも同じZod schemaで再検証する。

## 4. レンダリング戦略

- Suspense を積極的に使う。
- 可能な範囲でデータ待ち前に表示可能なUIを先に出す。
- ルート単位のローディングUIを `loading.tsx` で提供する。

## 5. キャッシュと状態管理

- 取得済みエンティティをクライアント状態ストアへ複製保存しない。
- データキャッシュは Next.js cache を主軸にする。
- Zustand は UIの狭い状態（開閉、テーマ等）に限定する。
- フィルター/ソートはURLクエリパラメータで表現する。

## 6. i18n戦略

- `next-intl` を使う。
- ロケールメッセージは TypeScript で管理する。
- ロケールコードを標準化して扱う。
- ロケール判定は `middleware` と `cookie` を使う。
- デフォルトは locale パスを使わない（cookie のみで管理する）。
- locale パス（`/[locale]/...` や `localePrefix`）は SEO 要件が明示された場合のみ許可する。
- 管理画面（admin/backoffice/dashboard）は locale パスを禁止し、常に cookie 管理のみとする。
- 必要なロケールバンドルのみを読み込む。

## 7. UIとスタイル規約

- スタイリングは Tailwind CSS を使う。
- 独自 `className` を導入する場合は BEM 命名に従う。
- UI部品は shadcn/ui を利用する。
- 共有UIは `shared/ui/` に配置する。

## 8. FSDレイヤー規約

- FSDベースのコンポーネント設計を採用する。
- `Entities` レイヤーは導入しない。
- 上位レイヤーへの import を禁止する。
- 同一レイヤーからの import は許可する。

## 9. 簡単なコーディング例

```tsx
// app/users/page.tsx (Server Component)
import { z } from "zod";
import { apiClient } from "@/shared/lib/apiClient";

const usersSchema = z.array(z.object({ id: z.string(), name: z.string() }));

async function fetchUsers() {
  const res = await apiClient.get("/users");
  return usersSchema.parse(res.data);
}

export default async function UsersPage() {
  const users = await fetchUsers();
  return <ul>{users.map((u) => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

```ts
// app/users/actions.ts (Server Action)
"use server";

import { revalidatePath } from "next/cache";

export async function createUser(input: { name: string }) {
  // 実際はサーバー側でZod再検証してから保存する
  await fetch(`${process.env.API_BASE_URL}/users`, {
    method: "POST",
    body: JSON.stringify(input),
    headers: { "Content-Type": "application/json" },
  });
  revalidatePath("/users");
}
```

## 10. 必須コンプライアンスチェック

実装またはレビューごとに `references/compliance-checklist.md` を実行する。
1項目でも違反があれば報告し、修正完了まで終了しない。
