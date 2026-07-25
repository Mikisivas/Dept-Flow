# Component conventions — shadcn/ui

Source: `shadcn-ui/ui`. Applies to all UI work in this project (Next.js / React /
TypeScript).

## What shadcn/ui is

Not a traditional npm component library you install and treat as a black box — a
**code-distribution model**. The CLI copies component source directly into this
repo (typically `components/ui/`), so the project owns and can freely edit every
component's code. Prefer this over pulling in a heavier all-in-one UI kit.

## Stack it assumes

- **React** + **TypeScript**
- **Tailwind CSS** for styling
- **Radix UI** primitives underneath for accessible interactive behavior
  (dialogs, dropdowns, popovers, etc. — Radix supplies the ARIA/keyboard behavior,
  shadcn supplies the styling on top)

## Setup and adding components

```bash
npx shadcn@latest init      # one-time: sets up tailwind config, cn() util, css variables
npx shadcn@latest add button dialog form   # add specific components as needed
```

Only add the components actually needed for the screen being built — don't bulk-add
the whole catalog speculatively.

## Conventions to follow in this repo

- **`cn()` utility** (`clsx` + `tailwind-merge`) for conditionally combining class
  names — use it instead of manual string concatenation or template literals for
  class lists.
- **`cva` (class-variance-authority)** for components with variants (size, intent,
  state) — define variants declaratively rather than branching JSX per variant.
- **Theming via CSS variables** (e.g., `--background`, `--primary`) set at the root,
  not hardcoded hex values scattered through components — this is what makes
  dark-mode/light-mode theming (see the interface guidelines' Dark Mode section)
  consistent.
- **Own the code.** Once a shadcn component is added to this repo, treat it as this
  project's own source, not a vendored dependency — edit it directly when a screen
  needs different behavior, rather than wrapping it in another abstraction layer.
- **Compose, don't reinvent.** Build higher-level Dept-Flow components (e.g., a
  `ComplianceStatusBadge`, a `CheckpointTokenInput`) out of these primitives rather
  than writing raw HTML/ARIA handling from scratch — the primitives already satisfy
  most of the accessibility checklist (focus management, keyboard nav, ARIA roles).
