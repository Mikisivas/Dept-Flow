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
  --primary:              var(--brand);        /* #F0952B — the crest orange */
  --primary-foreground:   var(--ink);          /* BLACK on orange — 8.52:1 */
  --accent:               var(--brand-tint-2);
  --accent-foreground:    var(--ink);
  --destructive:          var(--danger);
  --destructive-foreground: #FFFFFF;
  --muted-foreground:     var(--muted);
  --border:               var(--line);
}
```

**`--primary-foreground` must be black, not white.** shadcn's default button puts
`--primary-foreground` on `--primary`; leaving that as white would render white text
on `#F0952B` at **2.32:1** — a severe failure. Black on the crest orange is 8.52:1,
and it matches how the logo itself sets the SAMACOSS wordmark. Override this token
during `init` and verify the first button you build.
