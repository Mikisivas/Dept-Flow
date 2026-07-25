# Accessibility and interface-quality checklist

Source: adapted from `vercel-labs/agent-skills` → `skills/web-design-guidelines`
(whose rule set is drawn from `vercel-labs/web-interface-guidelines`). Run every
non-trivial screen against this before calling it done.

> **Note on sourcing:** a fifth requested source — a dedicated web-accessibility
> skill at `supercent-io/skills-template` — returned a 404 at build time (repo not
> reachable). This checklist's Accessibility and Focus States sections cover the
> same ground (WCAG-aligned keyboard/ARIA/focus rules) and stand in for it. If you
> have a working URL for that source, it can be merged in here.

## Accessibility
- Icon-only buttons require `aria-label`.
- Form controls need a `<label>` or `aria-label`.
- Interactive elements need keyboard handlers (`onKeyDown`/`onKeyUp`), not just
  `onClick`.
- Use `<button>` for actions, `<a>`/`<Link>` for navigation — never a
  `<div onClick>`.
- Images need `alt` (`alt=""` if purely decorative).
- Decorative icons need `aria-hidden="true"`.
- Async updates (e.g., a payment status changing) need `aria-live="polite"`.
- Prefer semantic HTML before reaching for ARIA attributes.
- Headings follow hierarchical structure `<h1>`–`<h6>`; include a skip link.
- Heading anchors need `scroll-margin-top` so they don't hide under a sticky header.

## Focus states
- Every interactive element needs a visible focus indicator via `focus-visible:ring-*`.
- Never use `outline-none` without a replacement focus style.
- Prefer `:focus-visible` over `:focus` (avoids showing rings on mouse clicks).
- Use `:focus-within` for compound controls (e.g., a labeled input group).

## Forms
(Especially relevant to Dept-Flow's registration, payment, and grace-period forms.)
- Inputs need `autocomplete` and a meaningful `name`.
- Use the correct `type` (`email`, `tel`, `url`, `number`) and `inputmode`.
- Never block paste with `onPaste` + `preventDefault` (e.g., don't block pasting an
  OTP code).
- Labels must be clickable (`htmlFor` or wrapping the control).
- Disable spellcheck on emails, OTP codes, matric numbers, usernames.
- Checkboxes/radios: label and control share a single hit target.
- Submit buttons stay enabled until the request actually starts (don't disable
  prematurely — students on flaky connections need to be able to retry a tap).
- Errors render inline; focus moves to the first error on submit.
- Placeholders end with `…` and show an example pattern (e.g., matric number
  placeholder shows the real format).
- Use `autocomplete="off"` on non-auth fields.
- Warn before navigation away from a form with unsaved changes.

## Animation
- Honor `prefers-reduced-motion`.
- Animate only `transform`/`opacity` (cheap, GPU-friendly — matters for
  low-end/low-bandwidth devices).
- Never use `transition: all`.
- Set the correct `transform-origin`.
- SVG transforms go on a `<g>` wrapper with `transform-box: fill-box`.
- Animations must be interruptible (a student tapping through quickly shouldn't
  get stuck mid-animation).

## Typography
- Use an ellipsis character `…`, not three periods `...`.
- Use curly quotes `"` `"`, not straight quotes.
- Use non-breaking spaces for measurements and brand names (e.g., "30 m", "Dept-Flow").
- Loading states end with `…` ("Verifying payment…").
- Use `font-variant-numeric: tabular-nums` for number columns (attendance %, amounts).
- Use `text-wrap: balance` or `text-pretty` on headings.

## Content handling
- Text containers handle long content via `truncate`, `line-clamp-*`, or
  `break-words` (course names, student full names vary a lot in length).
- Flex children need `min-w-0` to allow truncation to actually work.
- Handle empty states explicitly ("No sessions recorded yet" rather than a blank
  table).
- Anticipate varied user-generated content lengths throughout.

## Images
- `<img>` needs explicit `width` and `height` to avoid layout shift.
- Below-fold images use `loading="lazy"`.
- Critical above-fold images use `priority` (Next.js) or `fetchpriority="high"`.

## Performance
(Weight heavily — Dept-Flow's stated environment is low-bandwidth, mobile-first,
with burst concurrency during checkpoint windows.)
- Lists over ~50 items require virtualization (e.g., a department-wide student
  roster).
- No layout reads during render.
- Batch DOM reads/writes.
- Prefer uncontrolled inputs where practical.
- Add `<link rel="preconnect">` for third-party CDNs.
- Critical fonts use `<link rel="preload">` with `font-display: swap`.

## Navigation & state
- URL reflects state — filters, tabs, pagination, open panels belong in query
  params so a screen is shareable/refreshable.
- Use `<a>`/`<Link>` for anything that's actually navigation.
- Deep-link stateful UI (e.g., a specific student's risk detail view).
- Destructive actions (deactivate student, revoke registration) need confirmation
  or an undo window — these map directly to the admin/HOD actions that also require
  audit logging.

## Touch & interaction
(Primary interface is mobile.)
- Use `touch-action: manipulation` to avoid double-tap-to-zoom delay.
- Set `-webkit-tap-highlight-color` intentionally rather than leaving the default.
- Use `overscroll-behavior: contain` in modals/drawers (e.g., the checkpoint token
  entry sheet).
- Disable text selection during drag interactions.
- Use `autoFocus` sparingly — desktop only, avoid on mobile (it can pop the
  keyboard unexpectedly and shift layout).

## Safe areas & layout
- Full-bleed layouts need `env(safe-area-inset-*)` for notches/home indicators.
- Avoid unwanted scrollbars.
- Prefer Flex/Grid layout over JS-based measurement.

## Dark mode & theming
- Set `color-scheme: dark` on `<html>` when in dark mode.
- Match `<meta name="theme-color">` to the background.
- Native `<select>` elements need explicit `background-color` and `color` set (they
  don't inherit theme correctly by default).

## Locale & i18n
- Use `Intl.DateTimeFormat` for dates/times (relevant for session timestamps,
  Day-30/31 boundaries).
- Use `Intl.NumberFormat` for numbers/currency (dues amounts in Naira).
- Detect language via headers, not IP.
- Wrap identifiers (matric numbers, course codes) with `translate="no"`.

## Hydration safety (Next.js/React)
- Inputs with a `value` prop need `onChange` (or use `defaultValue` for
  uncontrolled).
- Guard against date/time hydration mismatches (server vs. client clocks —
  especially relevant near the Day-30/31 midnight boundary).
- Use `suppressHydrationWarning` minimally and only where justified.

## Hover & interactive states
- Buttons/links need a `hover:` state.
- Interactive states should increase contrast, not just add decoration.

## Content & copy
- Use active voice.
- Apply Title Case to headings/buttons.
- Use numerals for counts ("12 classes," not "twelve").
- Write specific button labels ("Pay dues," not "Continue").
- Error messages include the fix or next step, not just what went wrong.
- Use second person ("Your attendance is at 72%").
- Use `&` when space-constrained (mobile nav labels).

## Anti-patterns — flag and reject these on sight
- `user-scalable=no` or `maximum-scale=1` in the viewport meta tag.
- `onPaste` with `preventDefault`.
- `transition: all`.
- `outline-none` without a replacement focus style.
- Inline `onClick` used for navigation.
- `<div>`/`<span>` with a click handler instead of a real interactive element.
- Images without explicit dimensions.
- Large arrays/lists rendered without virtualization.
- Form inputs without labels.
- Icon buttons without `aria-label`.
- Hardcoded date/number formats instead of `Intl.*`.
- Unjustified `autoFocus` (especially on mobile).
