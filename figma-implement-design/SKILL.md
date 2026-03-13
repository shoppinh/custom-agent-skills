---
name: figma-implement-design
description: Implement UI from Figma designs. Use when the user is assigned a task to implement a design from Figma, build a screen from a Figma file, or translate a Figma mock into code. Requires plan-first workflow, fetch design via Framelink MCP, map to design system, produce an implementation plan for review, then execute only after user replies "APPROVE PLAN". Styling must follow docs/styling-guidelines.md 
metadata:
  internal: true
---

# Figma Implement Design

## Overview

This skill guides implementing Figma designs in (Next.js, TypeScript, styled-components + twin.macro + MUI). It uses **Framelink MCP for Figma** to fetch design data, maps the design to the project’s design system and styling rules, produces an implementation plan for your review, and implements only after you reply **"APPROVE PLAN"**. All styling decisions **must** follow **docs/styling-guidelines.md**.

## Styling rule (mandatory)

**Follow **docs/styling-guidelines.md** for every styling decision.** It defines:

- **Layer 1 — Custom design system:** `styled-components` + `twin.macro` for `H`-prefixed components; use theme tokens via `${(p) => p.theme.*}`.
- **Layer 2 — Admin widgets:** `@mui/material` for data tables, selects, dialogs, form inputs.
- **Layer 3 — Layout and utility:** Tailwind via `twin.macro` (`tw\`...\``) for page layout, spacing, flex/grid.

Do not invent design tokens; use only theme tokens and components that exist in the codebase or are documented in the design system / component library docs (if present). Reuse existing components first.

## Workflow (mandatory)

**Follow these phases in order. Do not skip phases.**

### Phase 1 — Analyze Figma
- Use the **official Figma MCP** first when planning or coding.
- If you get a timeout or rate limit error (common with free tier seats), **fallback to Framelink MCP** by calling the tool with `server`: `user-Framelink MCP for Figma`.
- **`get_figma_data`** with:
  - `fileKey`: from the Figma URL `figma.com/(file|design)/<fileKey>/...`
  - `nodeId`: if the task targets a specific frame/component (from `node-id=<nodeId>` in the URL); use format `1234:5678` or multiple nodes as `I5666:180910;1:10515`.
- If the design includes images or icons to export, use **`download_figma_images`** once you have node IDs and file names from the Figma data (pass `fileKey`, `nodes`, and `localPath`).
- From the response, **extract**: layout, components, typography, spacing, and interactions.

### Phase 2 — Map to design system

- Map the Figma design **only** to tokens and components that are defined in:
  - **docs/design-system.md** and **docs/component-library.md** (if they exist in the repo),
  - Otherwise: theme and `H`-prefixed components in the codebase and the rules in **docs/styling-guidelines.md**.
- Do not invent new design tokens or new component APIs; reuse existing components first and only add new ones when no existing component fits and the plan documents it.

### Phase 3 — Generate implementation plan

Produce a **step-by-step** implementation plan that includes:

- **Files to create or modify** (paths under `src/` following project folder structure).
- **Components to reuse** (existing `H`-prefixed or MUI components) and where they are used.
- **Props structure** for any new or wrapped components.
- **Responsive behavior** (breakpoints, layout changes).
- **Target location**: page/route, container, or new component path.
- **Styling approach** per **docs/styling-guidelines.md**: which layer (styled-components+twin, MUI, or Tailwind utilities) for each part.
- **Assets**: any SVGs/PNGs from `download_figma_images` and where they live.
- **Data/state**: if the screen is dynamic (Redux, local state, BFF).

### Phase 4 — Pause for approval

- Output the plan clearly (e.g. numbered steps and file list).
- **Do not create or edit any project files yet.**
- Ask explicitly: *"Review this plan. Reply with **APPROVE PLAN** to proceed with implementation, or describe changes you want."*
- Proceed to Phase 5 **only** when the user replies with **"APPROVE PLAN"** (or equivalent explicit approval). If they request changes, update the plan and repeat Phase 4 until they approve.

### Phase 5 — Implement

- Generate code **only after** approval.
- Follow:
  - **docs/ARCHITECTURE.md** (or **docs/frontend-architecture.md** if it exists) for structure and folder conventions.
  - **docs/styling-guidelines.md** for all styling (mandatory).
  - **docs/coding-standards.md** for TypeScript, naming, state, i18n, and project conventions (use when present).
- Apply **React/Next.js performance best practices**: avoid request waterfalls, prefer direct imports and `next/dynamic` for heavy components where appropriate, and keep client state/re-renders in check. Consider the **vercel-react-best-practices** skill when implementing data flow, bundle size, or re-renders.
- Create/update files, add assets, and match the approved plan. Follow project folder structure; reuse existing components first.

## Constraints

- **Do not invent design tokens** — use only theme tokens and design system values that exist in the codebase or docs.
- **Reuse existing components first** — prefer `H`-prefixed and MUI components; add new components only when necessary and document in the plan.
- **Follow project folder structure** — `src/pages/`, `src/containers/`, `src/components/` and conventions in `docs/ARCHITECTURE.md`.

## Project alignment

- **Stack:** Next.js 13 (Pages Router), TypeScript, Redux Toolkit + redux-saga, styled-components, twin.macro, MUI, Tailwind.
- **Styling:** Always apply **docs/styling-guidelines.md** for every styling decision.

## Resources

- **docs/ARCHITECTURE.md** — Folder structure and architecture.
- **docs/design-system.md**, **docs/component-library.md** — Use when present for tokens and reusable components.
- **docs/styling-guidelines.md**, **docs/coding-standards.md** — Styling (mandatory) and project coding conventions.
- **vercel-react-best-practices** skill — Consider when implementing data fetching, bundle size, re-renders, or server/client performance.
