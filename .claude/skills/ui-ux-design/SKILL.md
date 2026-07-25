---
name: ui-ux-design
description: Use whenever building or reviewing frontend/UI work for this project — creating pages or components, choosing colors/typography/layout, picking or wiring up shadcn/ui components, or auditing an interface for accessibility and interaction-design quality. Merges a subject-grounded design process, a deliberate style/palette selection method, shadcn/ui component conventions, and a full accessibility + interface-guidelines checklist into one workflow. Triggers on "design this page", "build this component", "pick a color palette", "review this UI for accessibility", "add a shadcn component".
---

# UI/UX Design — merged workflow

This skill combines five sources into one process for Dept-Flow's frontend
(Next.js / React / TypeScript, mobile-first, low-bandwidth-tolerant — see
`docs/system-operation-and-logic.md` for the product context):

1. **Design process & taste** — grounding a design in its actual subject matter and
   avoiding generic AI-default looks (adapted from Anthropic's `frontend-design` skill).
2. **Deliberate style/palette selection** — choosing an explicit, justified design
   system rather than defaulting (adapted from the *ui-ux-pro-max* approach of
   naming a style archetype, palette, and type pairing up front).
3. **Component conventions** — how components are built and owned in this stack
   (shadcn/ui: React + Tailwind + Radix primitives).
4. **Accessibility and interface-quality rules** — a concrete, checkable rule set
   (adapted from Vercel's web-interface-guidelines, which doubles as the
   accessibility checklist).

Full detail for each lives in `reference/` — read the relevant file before doing the
matching step below, rather than relying on this summary alone.

## Workflow

**1. Ground the design in the subject, don't default to it.**
Before picking anything, name the concrete thing being designed and who it's for
(e.g., "the HOD's grace-period override panel — a once-a-week action by one
authority figure who needs to trust it," not "an admin panel"). See
`reference/design-principles.md` → *Ground it in the subject*.

**2. Choose and state a design system explicitly — don't let one emerge by accident.**
Name: a style direction, a 4–6 color palette (as hex values), a type pairing
(display + body, + a utility face for data/timestamps if needed), and one signature
element this screen will be remembered by. Then check the choice isn't one of the
three generic AI-default looks called out in `reference/design-principles.md`. Keep
this proportionate — a student-facing attendance screen doesn't need the same
design investment as a marketing page.

**3. Build with shadcn/ui components, following this repo's conventions.**
See `reference/component-conventions.md` for install commands, the `cn()` /
`cva` patterns, and the "own the code, don't lock it behind a dependency" model.

**4. Before calling any UI done, run it against the interface-guidelines checklist.**
See `reference/accessibility-and-interface-guidelines.md` — this is the mandatory
pre-ship checklist covering accessibility, focus states, forms, animation,
performance, and the explicit anti-pattern list. Given Dept-Flow's low-bandwidth,
mobile-first requirement, weight the **Performance**, **Touch & Interaction**, and
**Content Handling** sections especially heavily.

**5. Self-critique once before shipping.**
Take a screenshot if the environment supports it. Ask: does this look like the
generic default for this kind of screen, or a choice made for *this* one? Cut
anything decorative that doesn't serve the brief — see the *Restraint* note in
`reference/design-principles.md`.

## Source attribution

- Design process & copy principles: adapted from `anthropics/skills` →
  `skills/frontend-design`.
- Style/palette/anti-pattern selection method: adapted from the approach used by
  `nextlevelbuilder/ui-ux-pro-max-skill` (a design-system generator covering UI
  style archetypes, color palettes, font pairings, and industry-specific
  anti-patterns).
- Component conventions: `shadcn-ui/ui` (React + Tailwind + Radix, code-ownership
  model).
- Accessibility & interface checklist: adapted from
  `vercel-labs/agent-skills` → `skills/web-design-guidelines`, whose rule set
  (sourced from `vercel-labs/web-interface-guidelines`) covers accessibility,
  focus, forms, motion, and performance in one list.
- A fifth source, a web-accessibility skill at `supercent-io/skills-template`, was
  requested but the repository could not be reached (404) at build time — it is
  not included here. If you have a corrected URL, the accessibility reference file
  can be extended with it.
