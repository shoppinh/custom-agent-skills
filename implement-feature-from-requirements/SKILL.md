---
name: implement-feature-from-requirements
description: Implement feature logic and data flow from requirements in a React/Next.js app. Use when the user asks to implement a feature, add business logic, wire up APIs, add state management, data fetching, BFF routes, Redux/saga, or backend integration. Excludes UI/design implementation (use figma-implement-design for that). Requires plan→review→user confirm→execute→review→user confirm workflow. Apply vercel-react-best-practices for data fetching and performance.
metadata:
  internal: true
---

# Implement Feature From Requirements

## Scope

**In scope:** Logic, state (Redux/saga), data fetching, BFF/API wiring, types, selectors, apiHelper, error handling, i18n keys for messages. **Out of scope:** Visual design, layout, styling, new UI components, Figma implementation — do not change styles or design tokens unless the requirement explicitly affects behaviour (e.g. conditional display).

## Workflow (mandatory)

Follow these phases in order. Do not skip phases.

### Phase 1 — Plan

From the requirement, produce an **implementation plan** that covers only logic and data:

- **Domain:** Which feature/domain (slice name, saga, selectors).
- **State:** New slice state shape, actions, and where they are used.
- **Data flow:** Which APIs are called; from where (saga, container, page); BFF route(s) if new; api + apiHelper + BFF layout per project (see docs/ARCHITECTURE.md).
- **Types:** New or updated types (co-located or in `src/types/`).
- **Side effects:** Sagas (takeLatest/takeEvery), navigation, error handling, i18n for user-facing messages.
- **Performance:** Avoid waterfalls (parallelise independent fetches); prefer direct imports; use vercel-react-best-practices for data fetching and re-renders.

Do **not** include UI/styling steps. Do not create or edit files yet.

### Phase 2 — Review (plan)

Present the plan clearly (e.g. numbered steps, file list, data-flow sketch). Ask the user to review.

### Phase 3 — Confirm (plan)

Wait for explicit user confirmation (e.g. "approved", "go ahead", "looks good") before implementing. If the user requests changes, update the plan and repeat Phase 2–3 until they confirm.

### Phase 4 — Execute

Implement **only after** Phase 3 confirmation. Create/update only the files in the plan (slices, sagas, selectors, services/api, services/apiHelper, pages/api, types, container/page logic). Follow:

- **docs/ARCHITECTURE.md** — Folder structure, BFF, "Adding a New Feature Domain".
- **docs/coding-standards.md** — TypeScript, naming, state, i18n, imports, performance.
- **vercel-react-best-practices** skill — Data fetching (no waterfalls, parallelise, SWR/dedup if applicable), bundle (direct imports, next/dynamic for heavy modules), re-renders (deps, memo, functional setState).

Do not add or change styling, theme tokens, or layout unless the requirement explicitly requires it.

### Phase 5 — Review (implementation)

Present a short **implementation summary**: what was added/changed (files, main actions, API flow). Optionally note any follow-ups (e.g. tests, i18n keys to add to locale files). Ask the user to review.

### Phase 6 — Confirm (implementation)

Wait for user confirmation. If they request changes, make edits and repeat Phase 5–6 as needed.

## Project alignment

- **Stack:** Next.js 13 (Pages Router), TypeScript, Redux Toolkit, redux-saga, redux-injectors, BFF (NextApiClient → pages/api → ApiClient).
- **New feature:** Slice + `use*Slice()` + saga + selectors; api + apiHelper; BFF route if new backend call. See ARCHITECTURE.md "Adding a New Feature Domain".
- **No UI/design:** Do not create new styled-components, theme tokens, or design-system components in this skill; only logic and data.

## Resources

- **docs/ARCHITECTURE.md** — Data flow, folder structure, BFF, adding a feature.
- **docs/coding-standards.md** — TypeScript, naming, state, i18n, performance.
- **vercel-react-best-practices** skill — Apply when implementing data fetching, bundle size, re-renders, and server/client performance.
