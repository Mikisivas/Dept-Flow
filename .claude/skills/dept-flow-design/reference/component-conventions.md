# Component conventions — shadcn/ui

Source: `shadcn-ui/ui`. Stack: Next.js / React / TypeScript / Tailwind.

## The model

shadcn/ui is not a black-box npm component library — the CLI **copies source into
this repo** (`components/ui/`), so the project owns and edits every component.
Prefer it over a heavier all-in-one kit: smaller bundles matter on cellular data.

Underneath, **Radix UI** primitives supply accessible behavior (focus trapping,
keyboard navigation, ARIA roles) for dialogs, dropdowns, popovers, and sheets. That
is most of the accessibility checklist handled for free — which is exactly why
these components should be composed rather than hand-rolled.

## Setup

```bash
npx shadcn@latest init                       # tailwind config, cn() util, CSS variables
npx shadcn@latest add button dialog form sheet badge table
```

Add only what a screen needs. Don't bulk-install the catalog.

## Conventions

- **`cn()`** (`clsx` + `tailwind-merge`) for conditional classes — never manual
  string concatenation.
- **`cva`** for variants. Dept-Flow's status components are the obvious case:
  define `StatusBadge` variants (`confirmed | provisional | pending | locked |
  atRisk`) declaratively rather than branching JSX.
- **Theme through CSS variables** from `reference/color-and-type.md`, mapped onto
  shadcn's token names in `globals.css`. Never hardcode hex values inside
  components — dark mode and any future palette change both depend on this.
- **Own the code.** Once a component is in `components/ui/`, edit it directly.
  Don't wrap it in another abstraction layer to avoid touching it.
- **Compose Dept-Flow components from primitives:** `CheckpointStrip`,
  `StatusBadge`, `AttendanceMeter`, `TokenEntrySheet`, `GraceOverrideDialog` all
  build on shadcn/Radix rather than raw HTML with hand-written ARIA.

## Mapping the palette onto shadcn tokens

```css
/* globals.css */
:root {
  --primary:              var(--brand-strong);  /* #C2410C — white text passes */
  --primary-foreground:   #FFFFFF;
  --accent:               var(--brand);         /* #EA580C — black text only */
  --accent-foreground:    var(--ink);
  --destructive:          var(--danger);
  --muted-foreground:     var(--muted);
  --border:               var(--line);
}
```

Note `--primary` maps to **Deep Orange**, not Signal Orange — because shadcn's
default button puts `--primary-foreground` (white) on `--primary`, and white on
`#EA580C` fails contrast. This mapping is what keeps the default button accessible.
