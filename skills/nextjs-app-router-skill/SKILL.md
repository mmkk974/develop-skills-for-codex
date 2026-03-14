---
name: nextjs-app-router-skill
description: Next.js App Router の設計・実装ルールを強制するスキル。Use when creating, reviewing, or refactoring Next.js codebases that require strict server/client boundaries, Zod validation, Suspense-first rendering, route-handler + TanStack Query CRUD, next-intl i18n, Tailwind/shadcn UI, and FSD layering with explicit Entities boundaries.
---

# Next.js App Router Skill

このファイルのルールを実装・レビュー時に必ず適用する。

## 1. データ取得と更新の責務分離

- Client Component から直接APIを呼ばない。
- 初期データ取得は Server Component で実行する。
- データ取得関数は利用する Server Component / feature の近くにコロケーションする。不要に最上位へ引き上げない。
- ただし、複数の子コンポーネントが同じリソースを使う場合は親の page / layout 側に集約し、同一データの二重取得を禁止する。
- 読み取り関数は `fetchHogehoge` 形式の `async` 関数で実装する。
- 取得処理は `shared/lib/apiClient` の `apiClient.get()` を使う。
- `apiClient.get()` は `fetch` ベースで実装し、Next.js の Request Memoization を壊さない前提で使う。
- APIベースURLは環境変数から取得する。
- APIレスポンスは Zod で検証する。
- 型は Zod schema から推論して利用する。
- 同一リクエスト内で同じ入力の読み取りを重複実行しない。安定した引数の `fetch` / 共有取得関数 / `cache()` を使って再利用する。
- ループ内の逐次 `fetch` による N+1 を禁止する。関連データは API / BFF / DB クエリで集約し、一括取得する。
- 独立した複数リソースの取得は直列 await ではなく並列化して待つ。
- 更新系は Server Actions で実装する。
- Server Actions 実行後は `revalidatePath` か `revalidateTag` で無効化する。

## 2. ユーザー起点CRUD

- ユーザー操作のCRUDは Route Handlers + TanStack Query で実装する。
- インタラクションUIは Client Component に置く。
- ダイアログ等での prop バケツリレーを禁止する。
- インタラクティブ部品は必要データを自分で TanStack Query 取得する。
- ただし、親の Server Component で既に取得済みのデータをクライアント側で再取得しない。更新後の再同期が必要な場合だけ Client fetch を許可する。

## 3. バリデーション戦略

- クライアントとサーバーで Zod schema を共有する。
- クライアント側は React Hook Form + Zod で検証する。
- サーバー側でも同じZod schemaで再検証する。

## 4. レンダリング戦略

- Suspense を積極的に使う。
- 可能な範囲でデータ待ち前に表示可能なUIを先に出す。
- ページ遷移をまたいで不変なシェル、ナビゲーション、タブ、フィルターパネルは `layout.tsx` に寄せ、不要な再レンダリングと再フェッチを防ぐ。
- 状態の保持や再利用が不要で、意図的に再マウントしたい場合だけ `template.tsx` を使う。
- 重いコンポーネントは個別の `<Suspense>` 境界で遅延させ、軽いセクションを先に Streaming SSR で返す。
- `<Suspense>` の fallback はスケルトン等の意味ある待機UIにする。
- ルート単位のローディングUIを `loading.tsx` で提供する。

## 5. キャッシュと状態管理

- 取得済みエンティティをクライアント状態ストアへ複製保存しない。
- データキャッシュは Next.js cache を主軸にする。
- Request Memoization が効く同一リクエスト内の読み取りでは、`no-store` や毎回変わるオプションを乱用して重複リクエストを発生させない。
- Zustand は UIの狭い状態（開閉、テーマ等）に限定する。
- フィルター/ソートはURLクエリパラメータで表現する。

## 6. i18n戦略

- `next-intl` を使う。
- ロケールメッセージは TypeScript で管理する。
- ロケールコードを標準化して扱う。
- `middleware` でロケール判定する。
- 必要なロケールバンドルのみを読み込む。

## 7. UIとスタイル規約

- スタイリングは Tailwind CSS を使う。
- 独自 `className` を導入する場合は BEM 命名に従う。
- UI部品は shadcn/ui を利用する。
- 共有UIは `shared/ui/` に配置する。

## 8. コンポーネント共通化とリファクタリング

- JSX の重複、ローディングUI、空状態、フォーム部品、mapper、fetch wrapper を放置せず、責務単位で積極的に共通化する。
- 同じ責務が複数箇所に現れたら、`shared/ui` `shared/lib` `features/*` `widgets/*` の適切な層へ抽出する。
- コピペした派生コンポーネントを増やすより、props・slot・composition で差分を吸収できる構造を優先する。
- 共通化のために Server Component を無理に Client Component へ落とさない。再利用性より server-first を優先する。

## 9. FSDレイヤー規約

- FSDベースのコンポーネント設計を採用する。
- `Entities` レイヤーは必要に応じて導入する。
- 複数 feature / widget から再利用される業務エンティティ単位の型、schema、formatter、表示断片、selector は `entities/*` に集約する。
- `Features` にはユースケース固有の操作、フォーム、mutation、画面依存の相互作用を置き、エンティティの基礎表現まで抱え込まない。
- 上位レイヤーへの import を禁止する。
- 同一ドメイン（同一スライス）内の import は直接 import を許可する。
- 同一レイヤーの別ドメイン、または他レイヤーへの import は Public API（バレルファイル）経由に限定する。
- deep import（`@/features/foo/ui/Button` のような内部実装への直接参照）を禁止する。
- 各ドメインの Public API は必要最小限のみ export し、不要な再公開を禁止する。

## 10. 簡単なコーディング例

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

## 11. 必須コンプライアンスチェック

実装またはレビューごとに `references/compliance-checklist.md` を実行する。
1項目でも違反があれば報告し、修正完了まで終了しない。
