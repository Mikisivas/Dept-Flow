---
name: dept-flow-design
description: The design system and frontend workflow for Dept-Flow. Use when building or reviewing any UI in this project — pages, components, dashboards, forms, status indicators — or when choosing colors, typography, or layout, adding shadcn/ui components, or auditing a screen before shipping. Encodes the departmental palette (orange/white/black), the checkpoint-pair visual signature, per-role screen patterns, and the accessibility rules this project must not break. Triggers on "design this page", "build this component", "what color should this be", "review this UI", "add a shadcn component".
---

# Dept-Flow design system

Frontend: Next.js / React / TypeScript, mobile-first, built to load on Nigerian
cellular data inside lecture halls. Product logic lives in
`docs/system-operation-and-logic.md` — read it before designing any screen that
touches attendance, payment, or compliance state.

## The design thesis

Dept-Flow is an **institutional instrument**, not a consumer app. It decides whether
a student sits an exam. It should read as precise, legible, and trustworthy —
tabular, high-contrast, low-decoration. When in doubt, choose the version that looks
like a well-made register rather than the version that looks like a startup landing
page.

**The signature element: the checkpoint pair.** Dept-Flow's one genuinely unusual
mechanic is that a lecture is scored `0 / 0.5 / 1.0` from two checkpoints. Make that
the recurring visual motif — every session renders as two cells:

```
▮▮  Full (1.0)      ▮▯  Half (0.5)      ▯▯  Absent (0)
```

Orange fills a captured checkpoint; an outline marks a missed one. This motif scales
from a single row in a list to a full semester strip on the student dashboard, and it
comes from the system's own logic rather than from a template. Spend the design's
boldness here and keep everything around it quiet.

## Palette — departmental orange, white, black

Full tokens, CSS variables, and dark-mode values: `reference/color-and-type.md`.

The palette is generated from the **SAMACOSS crest** (hue ≈32°) so the UI and the
logo read as one system.

| Token | Hex | Use |
|---|---|---|
| **Brand Orange** | `#F0952B` | Crest orange. Fills, the checkpoint motif, primary button. **Black text on it.** |
| **Orange Text** | `#A75F0C` | The only orange safe as text on white. |
| **Ink** | `#0A0A0A` | Primary text, dark surfaces. |
| **White** | `#FFFFFF` | Primary surface. |
| **Slate / Muted** | `#525252` / `#737373` | Secondary and tertiary text. |

**The one rule that is easy to get wrong:** brand orange on white is **2.32:1** — a
severe contrast failure. Never set text in `#F0952B` on white, and **never put white
text on orange**. Orange is a *fill carrying black text* (8.52:1 ✓) — which is
exactly how the crest sets "SAMACOSS". If orange must be text, it darkens to
`#A75F0C` (4.90:1 ✓). Every ratio is verified in `reference/color-and-type.md`.

**Orange is the brand, not a warning.** Because the departmental color is orange, it
cannot also mean "caution" — a risk alert in orange would disappear into the brand
furniture. Status gets its own scale, kept clear of the brand:

| State | Treatment |
|---|---|
| Confirmed / Cleared | Green `#15803D` |
| Provisional | **Neutral, dashed outline** — not a warning, just *not yet counted* |
| Pending verification | Blue `#1D4ED8` — the system is working, the student need do nothing |
| Locked | Red `#B91C1C`, filled |
| At risk (advisory) | Red **outlined**, not filled — lower emphasis than Locked |

Never encode a state by color alone: every status carries an icon or a text label
too, so it survives colour-blindness and a bad phone screen in a bright lecture hall.

## Workflow

1. **Name the screen's job and its one user.** "The HOD's grace-period override —
   a rare, high-consequence action by one authority figure who must trust it" beats
   "an admin panel." Consult `docs/system-operation-and-logic.md` for the real states
   involved.
2. **Check the screen pattern.** `reference/screen-patterns.md` has the per-role
   layouts (student, lecturer, HOD, admin) and the shared components —
   status badges, the checkpoint strip, the attendance meter, the token entry sheet.
   Reuse before inventing.
3. **Build with shadcn/ui.** Conventions, CLI, and the `cn()`/`cva`/theming patterns
   are in `reference/component-conventions.md`.
4. **Run the pre-ship checklist.** `reference/frontend-checklist.md` — accessibility,
   focus, forms, performance, and the anti-patterns to reject. Weight
   **Performance**, **Touch**, and **Offline/slow-network** hardest: this app is used
   on cellular data in a lecture hall, often by hundreds of students inside the same
   3–5 minute token window.
5. **Critique once before shipping.** Screenshot it. Would this read correctly to a
   student glancing at it for two seconds on a cracked screen in daylight? Cut any
   decoration that does not serve that.

## Non-negotiables for this project

- **Never invent a state.** The only compliance states are the ones in
  `docs/system-operation-and-logic.md`: provisional, confirmed, pending
  verification, locked, cleared. Don't add "partial," "warning," or "review" to the
  UI vocabulary.
- **Provisional must never look like confirmed.** A student with 12 provisional
  sessions has *nothing counted yet*. If the dashboard shows a reassuring number,
  the design has actively misled them. Show the provisional total distinctly, with
  the reason and the fix ("Pay your dues to lock these in").
- **Never show raw GPS coordinates in the UI.** Location is captured momentarily and
  purged; the interface shows pass/fail only.
- **Destructive and authority actions confirm.** Deactivating a student, revoking a
  registration, granting grace, submitting a manual attendance batch — all need a
  confirmation step and a reason field, because all of them write to the audit log.
- **No biometrics anywhere.** No fingerprint prompts, no selfie capture, no "verify
  your face" UI — it is not in the system and must not appear in a mockup.

## Sources

Adapted for this project from: Anthropic `frontend-design` (subject-grounded
process, copy principles, avoiding generic AI-default looks); `ui-ux-pro-max`
(explicit style/palette/anti-pattern selection); `shadcn-ui/ui` (component
conventions); Vercel `web-design-guidelines` / `web-interface-guidelines`
(accessibility and interface rule set). A fifth requested source,
`supercent-io/skills-template` web-accessibility, was unreachable (404) — its ground
is covered by the checklist's accessibility and focus sections.
