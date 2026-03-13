## implement-feature-from-requirements

Skill for implementing **feature logic and data flow** from requirements (Next.js, TypeScript, Redux Toolkit, redux-saga, BFF), with a strict plan→review→confirm→execute→review→confirm workflow. This skill is **logic-only** — UI and visual design are handled separately by `figma-implement-design`.

`$ npx skills add <your-skill-repo> --skill implement-feature-from-requirements`

`SKILL.md`: `.cursor/skills/implement-feature-from-requirements/SKILL.md`

---

## When to Apply

Use **implement-feature-from-requirements** when:

- **You have a feature request or user story** and need to implement the **logic, state, and data flow** 
- **You are wiring APIs or BFF routes**: adding or updating calls through `api`, `apiHelper`, and `pages/api` routes.
- **You are working on Redux / saga**: adding slices, actions, selectors, sagas, and side effects.
- **You are handling business rules and errors**: domain logic, branching, error handling, and i18n for user messages.

Avoid this skill when:

- **The work is purely visual/UI** (layouts, styling, new components, Figma implementation) — in that case, use **`figma-implement-design`**.
- **You are only refactoring styling or design tokens** without logic or data changes.
- **The task is infrastructure-only** (e.g. CI config, repo scripts) without feature-level logic.

---

## Scope

- **In scope**
  - Redux slices and sagas (via redux-injectors).
  - State shape, actions, selectors, and reducers.
  - BFF/data flow: `api`, `apiHelper`, `pages/api` routes, and integration with backend services.
  - Types and interfaces for requests, responses, and domain models.
  - Side effects: navigation, notifications, error handling, and i18n message keys.
  - Performance-sensitive data fetching patterns (no waterfalls, parallelise where possible).

- **Out of scope**
  - Visual design, layout, and styling.
  - Creating or modifying design tokens and design-system components.
  - Figma-based UI work (handled by `figma-implement-design`).

---

## Workflow Phases (Overview)

The skill enforces a **six-phase workflow** that must be followed in order:

| Phase | Name                      | Purpose                                                        |
|------:|---------------------------|----------------------------------------------------------------|
| 1     | Plan                      | Turn requirements into a detailed logic & data plan.          |
| 2     | Review (plan)             | Present the plan for review and feedback.                     |
| 3     | Confirm (plan)            | Wait for explicit approval before making code changes.        |
| 4     | Execute                   | Implement exactly according to the approved plan.             |
| 5     | Review (implementation)   | Summarise changes (files, flows) for review.                  |
| 6     | Confirm (implementation)  | Apply feedback and confirm final implementation.              |

These phases are described in detail in `SKILL.md` and are **mandatory** for all feature-logic work using this skill.

---

## Quick Reference

### 1. Plan (Phase 1)

From the requirement or user story, the plan must cover **only logic and data**, including:

- **Domain**
  - Which feature/domain slice is affected or needs to be created.
  - How it fits into the existing architecture (see `docs/ARCHITECTURE.md` "Adding a New Feature Domain").

- **State**
  - New or updated slice state shape and actions.
  - Where that state is read (selectors) and updated (reducers, sagas).

- **Data flow**
  - Which backend APIs are called and what the contracts look like.
  - Whether new BFF routes (`pages/api`) are needed, and how they call `ApiClient`.
  - How `api` and `apiHelper` functions are structured for the new behaviour.

- **Types**
  - Request/response DTOs and domain types (co-located or under `src/types/`).

- **Side effects**
  - Sagas (`takeLatest` / `takeEvery`, etc.), navigation after success/failure, error handling.
  - i18n keys for user-facing messages (success, error, warnings).

- **Performance**
  - Avoid data-fetching waterfalls: parallelise independent calls where possible.
  - Use **vercel-react-best-practices** for data fetching and re-renders (deduplication, caching, memoization patterns).

Do **not** include any UI/styling changes in this plan. Do **not** create or edit files yet.

### 2–3. Review & Confirm (Plan) — Phases 2–3

- **Phase 2 — Review (plan)**
  - Present the plan clearly: numbered steps, file lists, and a concise data-flow explanation.
  - Ensure reviewers can see exactly which slices, sagas, APIs, and types will be touched.

- **Phase 3 — Confirm (plan)**
  - Wait for explicit approval (e.g. “approved”, “go ahead”, “looks good”) before coding.
  - If changes are requested, update the plan and repeat Phase 2–3 until approved.

### 4. Execute (Phase 4)

- Implement **only what the plan describes**, focusing on:
  - Slices, sagas, selectors, services (`api`, `apiHelper`), BFF routes under `pages/api`, and types.
  - Container/page **logic** (hooking into Redux, triggering sagas), not styling.
- Follow the project docs:
  - `docs/ARCHITECTURE.md` for folder structure, BFF patterns, and feature-domain conventions.
  - `docs/coding-standards.md` for TypeScript, naming, state management, imports, i18n, and performance guidelines.
  - **vercel-react-best-practices** skill for data fetching, bundle size, and re-render optimisations.
- Do **not** modify styles, theme tokens, or layout unless the requirement explicitly demands a behaviour that requires it.

### 5–6. Review & Confirm (Implementation) — Phases 5–6

- **Phase 5 — Review (implementation)**
  - Provide a concise summary:
    - Files added/changed.
    - New or updated slices, sagas, selectors, and API/BFF flows.
    - Any follow-ups (tests, locale file updates, documentation).

- **Phase 6 — Confirm (implementation)**
  - Wait for user feedback and confirmation.
  - Apply requested changes and repeat the review/confirm cycle as needed.

---

## Key Constraints

- **Logic-only**: No new styled-components, theme tokens, or UI components via this skill.
- **Architecture-first**: Follow `docs/ARCHITECTURE.md` for where new domains, APIs, and BFF routes live.
- **Performance-aware**: Apply **vercel-react-best-practices** when designing data fetching and state updates.
- **Separation of concerns**: Use `figma-implement-design` for UI work; keep this skill focused on behaviour and data.

---

## Project Alignment

- **Tech stack**: Next.js 13 (Pages Router), TypeScript, Redux Toolkit, redux-saga, redux-injectors, BFF (NextApiClient → `pages/api` → `ApiClient`).
- **Feature pattern**: A typical new feature includes:
  - Slice + `use*Slice()` hook.
  - Saga + selectors.
  - `api` + `apiHelper` functions.
  - Optional new BFF route(s) for backend integration.

---

## How to Use This Skill in Practice

- **When you receive a task like** “Implement feature X to fetch and manage Y data”:
  - Invoke **implement-feature-from-requirements**.
  - Provide:
    - The **requirement/user story** and acceptance criteria.
    - Any **existing APIs or endpoints**, or backend contracts if known.
    - Relevant context (current slices, sagas, routes).
  - Let the skill:
    - Propose a detailed plan (domain, state, data flow, types, side effects).
    - Wait for your approval before making any code changes.

- **When extending an existing feature**:
  - Use the same workflow, but the plan should call out:
    - Which existing slices/sagas/API routes will change.
    - Any breaking changes to types or data contracts.

---

## Related Resources

- **Skill definition**: `.cursor/skills/implement-feature-from-requirements/SKILL.md`
- **Architecture**: `docs/ARCHITECTURE.md`
- **Coding standards**: `docs/coding-standards.md`
- **Performance**: `vercel-react-best-practices` skill
- **UI implementation**: `docs/figma-implement-design.md` and the `figma-implement-design` skill

