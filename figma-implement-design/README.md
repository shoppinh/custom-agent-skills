## figma-implement-design

Skill for implementing Figma designs in (Next.js, TypeScript, styled-components + twin.macro + MUI) with a plan-first workflow and strict adherence to the project design system and styling guidelines.

`$ npx skills add https://github.com/shoppinh/custom-agent-skills --skill figma-implement-design`

---

## When to Apply

Use **figma-implement-design** when:

- **You have a Figma URL and a task to build UI** (new page, screen, or component) from that design.
- **You need to translate a Figma mock into production-ready code** using the existing design system, theme tokens, and component library.
- **You must ensure styling consistency** with `docs/styling-guidelines.md`, `docs/design-system.md`, and `docs/component-library.md`.
- **You want a plan-first workflow**: analyze the design, map it to the design system, propose a step-by-step implementation plan, and only then generate code after the plan is approved.

Avoid this skill when:

- **No Figma design is involved** (e.g. pure backend or logic work).
- **You are only tweaking behavior** of an existing component without design changes.
- **You are implementing non-visual features** (state management, BFF endpoints, data fetching) — in those cases, prefer the feature/architecture-oriented skills.

---

## Workflow Phases (Overview)

The skill enforces a **five-phase workflow** that must be followed in order:

| Phase | Name                      | Purpose                                                     |
|------:|---------------------------|-------------------------------------------------------------|
| 1     | Analyze Figma             | Fetch design data via Framelink MCP and understand the UI. |
| 2     | Map to design system      | Map Figma elements to existing tokens and components.       |
| 3     | Generate implementation plan | Produce a detailed, file-level implementation plan.     |
| 4     | Pause for approval        | Present the plan and wait for explicit **APPROVE PLAN**.    |
| 5     | Implement                 | Write code and assets strictly according to the approved plan. |

These phases are described in detail in `SKILL.md` and are **mandatory** for all Figma-driven UI work.

---

## Quick Reference

### 1. Analyze Figma (Phase 1)

- **Use Framelink MCP before any coding.**
- Call `get_figma_data` with:
  - **`fileKey`**: extracted from the Figma URL (`figma.com/(file|design)/<fileKey>/...`).
  - **`nodeId`**: if the task targets a specific frame or component (`node-id=<nodeId>` in the URL, e.g. `1234:5678` or `I5666:180910;1:10515`).
- If you need assets (icons, images), call `download_figma_images` with `fileKey`, the relevant nodes, and `localPath`.
- From the response, focus on **layout, components, typography, spacing, and interactions**.

### 2. Map to Design System (Phase 2)

- **Never invent design tokens.** Use only:
  - Tokens and patterns from `docs/styling-guidelines.md`.
  - Components and tokens documented in `docs/design-system.md` and `docs/component-library.md` (when present).
  - Existing theme tokens and `H`-prefixed components in the codebase.
- Decide for each design element:
  - **Which existing component to reuse** (H-components, MUI widgets, shared atoms/molecules).
  - **Which styling layer applies**:
    - Layer 1: `styled-components` + `twin.macro` for `H`-prefixed components and design system primitives.
    - Layer 2: MUI components for data-heavy admin widgets (tables, selects, dialogs, form fields).
    - Layer 3: Tailwind via `twin.macro` for layout, spacing, and utility styles.

### 3. Generate Implementation Plan (Phase 3)

The plan must clearly specify:

- **Files to create or modify** under `src/` (pages, containers, components).
- **Existing components to reuse** and how they are composed.
- **Props and data model** for any new components or wrappers.
- **Responsive behavior** (breakpoints, layout changes, visibility rules).
- **Styling approach** per section (which layer and which tokens/components).
- **Assets** (paths to downloaded Figma images/icons and where they are used).
- **Data/state** considerations (Redux, local state, BFF calls) when the screen is dynamic.

### 4. Pause for Approval (Phase 4)

- Present the plan using **clear headings, numbered steps, and file lists**.
- **Do not edit or create any project files yet.**
- Ask the reviewer to respond with **APPROVE PLAN** (or explicit approval) or request changes.
- If changes are requested, update the plan and repeat this phase until approved.

### 5. Implement (Phase 5)

- Only after approval:
  - Implement exactly according to the plan and the project architecture docs.
  - Follow `docs/ARCHITECTURE.md` (and any frontend-architecture docs if present) for folder structure and routing.
  - Respect `docs/styling-guidelines.md` and `docs/coding-standards.md` for all styling and TypeScript/React conventions.
- Apply **React/Next.js performance best practices** (e.g. avoid waterfalls, use direct imports, `next/dynamic` for heavy components where appropriate), and consider the **vercel-react-best-practices** skill for data fetching and rendering concerns.

---

## Key Constraints

- **Do not invent design tokens**: rely only on existing theme tokens and design system values.
- **Reuse existing components first**: prefer `H`-prefixed components and MUI widgets; only create new components when nothing appropriate exists and the plan documents them.
- **Follow project structure**: keep files under `src/pages/`, `src/containers/`, `src/components/` as described in `docs/ARCHITECTURE.md`.
- **Align with styling rules**: always apply the three-layer styling model from `docs/styling-guidelines.md`.

---

## Project Alignment

- **Tech stack**: Next.js 13 (Pages Router), TypeScript, Redux Toolkit + redux-saga, styled-components, twin.macro, MUI, Tailwind.
- **Design system**: Centralized design tokens and `H`-prefixed components as documented in `docs/design-system.md` and `docs/component-library.md` (when present).
- **Styling**: All styling decisions must comply with `docs/styling-guidelines.md`.

---

## How to Use This Skill in Practice

- **When you receive a task like** “Implement this Figma screen”:
  - Invoke the **figma-implement-design** skill.
  - Provide the **Figma URL**, any **target route or page context**, and **requirements about behavior or data**.
  - Let the skill:
    - Fetch Figma data via Framelink MCP.
    - Map the design to the existing design system.
    - Propose a detailed implementation plan.
  - Review the plan. Once you are satisfied, reply with **APPROVE PLAN** so that implementation can begin.

- **When updating an existing screen to match a new Figma revision**:
  - Use the same workflow, but ensure the plan identifies **which components and files will change** and how existing behavior will be preserved.

---

## Related Resources

- **Skill definition**: `.cursor/skills/figma-implement-design/SKILL.md`
- **Architecture**: `docs/ARCHITECTURE.md`
- **Design and components**: `docs/design-system.md`, `docs/component-library.md`
- **Styling & coding standards**: `docs/styling-guidelines.md`, `docs/coding-standards.md`
- **Performance**: `vercel-react-best-practices` skill for React/Next.js performance guidance

