# Design principles and process

Adapted from Anthropic's `frontend-design` skill (process, taste, copy) and the
`ui-ux-pro-max` approach (explicit, reasoned style/palette selection).

## Ground it in the subject

Before designing anything, pin down: one concrete subject, its audience, and the
page's single job. Distinctive design choices come from the subject's own world —
its materials, its vernacular, its actual content — not from a generic template.
Build with the real content and subject matter throughout, not lorem-ipsum
placeholders that get "designed around" later.

## The hero is a thesis

Open with the most characteristic thing in the subject's world — a headline, a
number, a live demo, an interactive moment — not the default "big number, small
label, gradient accent" template unless that's genuinely the best answer for this
subject.

## Typography carries personality

Pair a display face and a body face deliberately, not the default pairing you'd
reach for on any project. Set a clear type scale with intentional weights and
spacing. The type treatment should be memorable, not a neutral container for text.

## Structure encodes meaning

Structural devices — numbering, eyebrows, dividers, labels — should encode
something true about the content, not decorate it. Numbered markers (01/02/03)
only belong where the content is actually sequential; question whether a
structural device makes sense before using it.

## Motion serves the subject

Think about *whether* animation serves this specific subject — a load sequence, a
scroll reveal, a hover micro-interaction — rather than scattering effects
everywhere. One orchestrated moment usually lands harder than several scattered
ones. Excess animation is one of the strongest tells of generic AI-generated
design; when in doubt, use less.

## Match complexity to the vision

A maximalist direction needs elaborate execution to hold together; a minimal
direction needs precision in spacing, type, and detail to avoid feeling unfinished.
Elegance is executing the *chosen* direction well, not choosing the safest one.

## Avoid the three generic AI-default looks

Calibrate against these three clusters and actively avoid landing on one by
default (as opposed to by deliberate choice matching the brief):
1. Warm cream background (~`#F4F1EA`) + high-contrast serif display + terracotta accent.
2. Near-black background + a single bright acid-green or vermilion accent.
3. Broadsheet layout — hairline rules, zero border-radius, dense newspaper columns.

All three are legitimate *if the brief calls for them*. The problem is landing on
one by default regardless of subject.

## Process: brainstorm → critique → build → critique again

1. **Brainstorm a compact design plan** before writing any code:
   - **Color** — 4–6 named hex values.
   - **Type** — 2+ roles: a characterful display face used with restraint, a
     complementary body face, and a utility face for captions/data/timestamps if
     needed.
   - **Layout** — a one-sentence concept plus an ASCII wireframe to compare options.
   - **Signature** — the single element this screen will be remembered by.
2. **Critique the plan against the brief** before building: does any part of it
   read as the generic default for this kind of page, rather than a choice made
   for *this* brief? Revise, and note what changed and why.
3. **Build** following the revised plan — derive every color/type decision from it.
   Watch CSS selector specificity: type-based selectors (`.section`) and
   element/class-based selectors (`.cta`) can silently cancel each other,
   especially around spacing between sections.
4. **Critique again** before shipping (see the Restraint section below).

## Deliberate style/palette selection (explicit system choice)

Rather than letting a look emerge by accident, explicitly name and justify, up
front, for every non-trivial screen:
- A **style direction** (e.g., minimalist, glassmorphism, neumorphism, brutalist,
  data-dense/utilitarian) — chosen for *this* product and audience, not a default.
- A **color palette** (4–6 named hex values) appropriate to the product category
  and the emotional register it needs (e.g., a compliance/enforcement dashboard
  reads differently than a marketing landing page).
- A **type pairing** matched to the same register.
- **Anti-patterns to actively avoid** for this category — e.g., a financial/
  compliance interface should avoid playful over-animation or low-contrast
  decorative text that undermines trust.
- A short **pre-delivery checklist** specific to the screen's job (does the payment
  status read unambiguously at a glance? does the risk alert stand out without
  looking alarmist?).

This matters most for Dept-Flow's higher-stakes screens (payment status, grace
overrides, risk alerts) where a design that "looks nice" but reads ambiguously is
a real functional defect, not just a taste issue.

## Restraint and self-critique

Spend boldness in one place — the signature element — and keep everything else
quiet and disciplined. Cut any decoration that doesn't serve the brief. Build to a
quality floor without announcing it: responsive down to mobile, visible keyboard
focus, reduced motion respected (all mandatory — see
`reference/accessibility-and-interface-guidelines.md`).

Before shipping, ask: does this look like the generic default for this kind of
screen, or a choice made for this one? Take a screenshot if the environment
supports it — a picture surfaces problems text review misses.

## Writing and copy

Words exist to make a design easier to understand and use — they are design
material, not decoration.

- **Write from the end user's perspective.** Name things by what people control and
  recognize, not by how the system is built — a student manages *their attendance*,
  not a `session_score` record.
- **Use active voice.** A control says exactly what happens when used: "Pay dues,"
  not "Submit." An action keeps its name through the whole flow — a button that
  says "Pay" produces a result that says "Paid," not "Transaction processed."
- **Handle empty states and errors deliberately.** Explain what went wrong and how
  to fix it, in the interface's voice. Errors don't apologize and are never vague.
  An empty state (e.g., "no classes scheduled yet") is an invitation to act, not a
  dead end.
- **Keep the register conversational and consistent.** Plain verbs, sentence case,
  no filler. Let each element do exactly one job — a label labels, an example
  demonstrates, nothing does double duty.
