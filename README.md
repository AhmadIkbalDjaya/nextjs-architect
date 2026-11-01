# Next.js Architect

Agent skill that refactors Next.js and React directories by splitting large components, extracting hooks and services, optimizing `"use client"` boundaries, and enforcing one-component-per-file with colocation best practices.

Built using the open [Agent Skills](https://agentskills.io) format.

## Features

- Scans a target directory and inventories all components, hooks, API calls, and their relationships.
- Splits large components into small, focused, single-responsibility components in `_components/`.
- Groups components with ≥3 exclusive sub-components into dedicated subdirectories inside `_components/`.
- Extracts stateful logic and side effects into custom hooks in `_hooks/`.
- Moves API calls into dedicated service files in `_services/`.
- Extracts shared TypeScript types into `_types/` when applicable.
- Optimizes `"use client"` boundaries — pushes client directives as deep as possible.
- Detects shared components used across multiple routes and suggests `_shared/` relocation.
- Creates barrel exports (`index.ts`) for clean imports.
- Presents a complete refactor plan and waits for explicit user approval before making changes.
- Verifies the result with type checks, linting, and build validation.

## Output Structure

```
target-directory/
  page.tsx
  _components/
    ComponentA.tsx
    ComponentB.tsx
    component-c/
      index.ts
      ComponentC.tsx
      SubComponentD.tsx
      SubComponentE.tsx
    index.ts
  _hooks/
    useFeatureData.ts
    index.ts
  _services/
    featureApi.ts
    index.ts
  _types/
    feature.types.ts
    index.ts
```

Directories prefixed with `_` are ignored by Next.js App Router routing.

## Requirements

- Node.js and npm to install the skill.
- A supported agent: OpenCode, Claude Code, or Codex.

## Install

Run from the target project root:

```sh
npx skills add AhmadIkbalDjaya/nextjs-architect -a opencode -a claude-code -a codex -y
```

Install for one agent by specifying only that agent. For example:

```sh
npx skills add AhmadIkbalDjaya/nextjs-architect -a opencode -y
```

Omit `-y` to choose installation options interactively. To update the skill, run the install command again.

## Usage

Invoke the `nextjs-architect` skill from within your project and tag the directory you want to refactor. The agent scans all files, builds a complete refactor plan with component splits, hook extractions, service extractions, and `"use client"` optimizations, then asks for confirmation before making any changes.

Example:

> Refactor the `app/dashboard/` directory using the nextjs-architect skill.

## Refactor Rules

| Trigger | Action |
|---|---|
| Component > 80 lines | Split into smaller components |
| JSX nesting > 3 levels | Extract inner blocks |
| > 1 component per file | Split into separate files |
| > 3 useState/useEffect | Extract to custom hook |
| Hook mixes > 2 concerns | Split into separate hooks per concern |
| Hook returns > 10 values | Candidate for split by concern |
| Hook re-exports types/utils | Remove re-exports, import directly |
| Inline handler > 5 lines | Move logic to hook |
| fetch/axios/API call in component | Move to `_services/` |
| `"use client"` without interactivity | Remove directive |
| Component used across routes | Suggest `_shared/` relocation |
| ≥3 exclusive sub-components | Group into subdirectory with private barrel |

## Skill Contents

- `SKILL.md` — inspection, proposal and confirmation gate, refactor execution, verification, and reporting workflow.
