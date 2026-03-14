# Compliance Checklist

Use this checklist for every implementation and review.

## A. Server/Client Responsibility

- [ ] No direct API calls from Client Components.
- [ ] Initial data is fetched in Server Components.
- [ ] Fetch helpers are colocated near the consuming Server Component/feature unless there is a clear shared consumer.
- [ ] Shared data needed by multiple children is aggregated in the parent page/layout instead of fetched redundantly in each child.
- [ ] Read functions follow `fetchHogehoge` async naming and return Promise.
- [ ] Mutations are implemented as Server Actions.
- [ ] Server Actions call `revalidatePath` or `revalidateTag`.

## B. API Client and Validation

- [ ] Data fetching uses `shared/lib/apiClient`.
- [ ] Read access uses `apiClient.get()`.
- [ ] `apiClient.get()` is fetch-based and preserves Next.js Request Memoization behavior.
- [ ] Base URL is read from environment variables.
- [ ] API response is validated with Zod.
- [ ] Types are inferred from Zod schema.
- [ ] Client and server share the same Zod schema.
- [ ] Client validates forms with React Hook Form + Zod.
- [ ] Server re-validates inputs with shared Zod schema.

## C. CRUD and UI Composition

- [ ] User action CRUD uses Route Handlers + TanStack Query.
- [ ] Dialog-like components do not receive fetched data via prop-bucket-relay.
- [ ] Dialog-like components fetch needed data by themselves.
- [ ] Client components do not re-fetch data already resolved in the parent server tree without a mutation-driven reason.

## D. Rendering and Loading UX

- [ ] Suspense boundaries are used where partial rendering is possible.
- [ ] Non-blocking sections render before slow fetch completion.
- [ ] Stable shells/navigation/filter panels are placed in `layout.tsx` when they should persist across navigation.
- [ ] `template.tsx` is used only when intentional remount/reset is required.
- [ ] Heavy sections are isolated behind `<Suspense>` boundaries to enable Streaming SSR.
- [ ] Suspense fallback UI is meaningful (for example skeletons instead of blank gaps).
- [ ] Route-level `loading.tsx` exists and is meaningful.

## E. Cache and State

- [ ] Fetched entity data is not duplicated into Zustand/store.
- [ ] Next.js cache is used for server data.
- [ ] Request Memoization is preserved for identical reads within the same request/render tree.
- [ ] No loop-based per-item fetch causes N+1; related data is batched or aggregated at the API/BFF/query layer.
- [ ] Independent data sources are parallelized instead of awaited serially.
- [ ] Zustand usage is limited to UI logic.
- [ ] Filter/sort conditions are encoded in query parameters.

## F. i18n

- [ ] i18n uses `next-intl`.
- [ ] Locale messages are implemented in TypeScript.
- [ ] Locale detection is implemented in middleware.
- [ ] Only required locale resources are loaded.

## G. UI/Design System

- [ ] Styling uses Tailwind CSS.
- [ ] Custom class names follow BEM.
- [ ] shadcn/ui components are used for shared primitives.
- [ ] Shared UI components are placed under `shared/ui/`.

## H. Refactoring and Reuse

- [ ] Duplicated JSX, loading/empty states, form parts, mappers, or fetch wrappers are extracted at an appropriate layer.
- [ ] Copy-paste component variants are replaced with props/slots/composition where the responsibility is the same.
- [ ] Reuse does not force unnecessary Client Component promotion; server-first boundaries are preserved.

## I. FSD

- [ ] FSD structure is applied.
- [ ] `Entities` is introduced when entity-scoped types, schema, UI fragments, or selectors are reused across multiple features/widgets.
- [ ] Entity-level representations are placed in `entities/*`, while use-case-specific actions/forms/mutations remain in `features/*`.
- [ ] No imports from upper layers.
- [ ] Direct import is used only inside the same domain (same slice).
- [ ] Cross-domain imports in the same layer use Public API (barrel) only.
- [ ] Imports to other layers use Public API (barrel) only.
- [ ] No deep import to another domain/layer internals.
- [ ] Public API exports are minimal and do not over-re-export internals.

## Output Format for Reviews

When reporting compliance:

1. List failed checks first.
2. Provide file paths and exact lines for each failure.
3. Propose the minimum patch set to restore compliance.
4. Confirm passed sections briefly.
