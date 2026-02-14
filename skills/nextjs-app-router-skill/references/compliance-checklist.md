# Compliance Checklist

Use this checklist for every implementation and review.

## A. Server/Client Responsibility

- [ ] No direct API calls from Client Components.
- [ ] Initial data is fetched in Server Components.
- [ ] Read functions follow `fetchHogehoge` async naming and return Promise.
- [ ] Mutations are implemented as Server Actions.
- [ ] Server Actions call `revalidatePath` or `revalidateTag`.

## B. API Client and Validation

- [ ] Data fetching uses `shared/lib/apiClient`.
- [ ] Read access uses `apiClient.get()`.
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

## D. Rendering and Loading UX

- [ ] Suspense boundaries are used where partial rendering is possible.
- [ ] Non-blocking sections render before slow fetch completion.
- [ ] Route-level `loading.tsx` exists and is meaningful.

## E. Cache and State

- [ ] Fetched entity data is not duplicated into Zustand/store.
- [ ] Next.js cache is used for server data.
- [ ] Zustand usage is limited to UI logic.
- [ ] Filter/sort conditions are encoded in query parameters.

## F. i18n

- [ ] i18n uses `next-intl`.
- [ ] Locale messages are implemented in TypeScript.
- [ ] Locale detection is implemented in middleware + cookie.
- [ ] Default routing does not use locale path (`/[locale]/...` is absent).
- [ ] Locale path is used only when explicit SEO requirements exist.
- [ ] Admin/backoffice/dashboard routes never use locale path and rely on cookie-only locale state.
- [ ] Only required locale resources are loaded.

## G. UI/Design System

- [ ] Styling uses Tailwind CSS.
- [ ] Custom class names follow BEM.
- [ ] shadcn/ui components are used for shared primitives.
- [ ] Shared UI components are placed under `shared/ui/`.

## H. FSD

- [ ] FSD structure is applied.
- [ ] `Entities` layer is not used.
- [ ] No imports from upper layers.
- [ ] Same-layer imports only when needed and coherent.

## Output Format for Reviews

When reporting compliance:

1. List failed checks first.
2. Provide file paths and exact lines for each failure.
3. Propose the minimum patch set to restore compliance.
4. Confirm passed sections briefly.
