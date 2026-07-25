# Pre-ship checklist

Adapted from Vercel's web-interface-guidelines, filtered and reweighted for
Dept-Flow: mobile-first, cellular data, burst traffic inside a 3–5 minute token
window, and legally-relevant state (attendance, payment, eligibility).

> A fifth requested source — a web-accessibility skill at
> `supercent-io/skills-template` — was unreachable (404) when this skill was built.
> The Accessibility and Focus sections below cover the same WCAG ground.

## Highest priority for this project

### Slow and failing networks
- Every submission (token, payment, manual batch) must survive a dropped connection:
  local optimistic state + retry, with an honest pending indicator.
- Never leave a student on an ambiguous screen after a payment — show
  "Checking payment…" until the webhook lands.
- Loading states end with `…` and say what is happening, not just "Loading."
- Skeletons over spinners for known layouts; avoid layout shift on arrival.

### Performance
- Lists over ~50 rows (student rosters, session logs) must be virtualized.
- Explicit `width`/`height` on every image; `loading="lazy"` below the fold.
- Preload the one critical font with `font-display: swap`; no second font file.
- No layout reads during render; batch DOM reads/writes.
- `<link rel="preconnect">` for the Paystack/CDN origins.

### Touch
- `touch-action: manipulation` (kills the 300ms double-tap delay).
- Minimum 44×44px tap targets — the token keypad and primary actions especially.
- `overscroll-behavior: contain` inside sheets and modals.
- Set `-webkit-tap-highlight-color` intentionally.
- Avoid `autoFocus` on mobile; it pops the keyboard and shifts layout.
- `env(safe-area-inset-*)` on full-bleed layouts.

## Accessibility
- Icon-only buttons need `aria-label`.
- Every form control needs a `<label>` or `aria-label`.
- Use `<button>` for actions, `<a>`/`<Link>` for navigation — never `<div onClick>`.
- Images need `alt` (`alt=""` if decorative); decorative icons `aria-hidden="true"`.
- Async state changes (payment confirming, checkpoint accepted) need
  `aria-live="polite"`.
- Semantic HTML before ARIA. Headings in order `<h1>`–`<h6>`. Include a skip link.
- **Never encode status by color alone** — icon or text label always accompanies it.
- The CheckpointStrip needs per-cell accessible labels; the motif alone is not
  sufficient.

## Focus
- Visible focus on every interactive element via `focus-visible:ring-*`.
- Never `outline-none` without a replacement.
- Prefer `:focus-visible` over `:focus`; `:focus-within` for compound controls.
- On form submit with errors, move focus to the first error.

## Forms
(Registration, OTP, token entry, payment, grace override.)
- **Never block paste** — this breaks OTP and token entry.
- Correct `type` and `inputmode`; `autocomplete="one-time-code"` for OTP.
- Disable spellcheck on matric numbers, OTP codes, tokens.
- Labels clickable (`htmlFor` or wrapping); checkbox/radio share one hit target.
- Submit stays enabled until the request actually starts — students on flaky
  connections need to retry.
- Errors render inline and state the fix.
- Placeholders show the real format and end with `…`.
- Warn before navigating away from unsaved input.

## Animation
- Honor `prefers-reduced-motion`.
- Animate only `transform`/`opacity`; never `transition: all`.
- Animations must be interruptible.
- The provisional→confirmed fill is the one place worth a deliberate animation.
  Everything else stays quiet.

## Typography and numbers
- `font-variant-numeric: tabular-nums` wherever numbers stack or update.
- `…` not `...`; curly quotes; non-breaking spaces in "30 m", "Dept-Flow".
- `text-wrap: balance` on headings.

## Content handling
- Long course names and student names need `truncate` / `line-clamp-*` /
  `break-words`; flex children need `min-w-0`.
- Every list has a designed empty state that states the next action.

## Navigation and state
- URL reflects state — filters, tabs, selected student, pagination in query params.
- Deep-link stateful views (a specific student's risk detail).
- **Destructive and authority actions confirm**: deactivation, revoke registration,
  grace period, manual batch, level rollover. All write audit logs; the UI must not
  make them feel casual.

## Locale
- `Intl.DateTimeFormat` for all dates/times — critical around the Day-30/31
  midnight boundary; never hardcode a format.
- `Intl.NumberFormat` for Naira amounts.
- `translate="no"` on matric numbers and course codes.

## Dark mode
- `color-scheme: dark` on `<html>`; `<meta name="theme-color">` matches surface.
- Native `<select>` needs explicit `background-color` and `color`.
- Brand orange switches to `#FB923C` on dark (see `color-and-type.md`).

## Hydration (Next.js)
- Inputs with `value` need `onChange`, or use `defaultValue`.
- Guard date/time hydration mismatches — server and client clocks differ, and this
  app has a hard midnight boundary.
- `suppressHydrationWarning` only where justified.

## Reject on sight
- `user-scalable=no` / `maximum-scale=1`
- `onPaste` + `preventDefault`
- `transition: all`
- `outline-none` with no replacement
- `<div>`/`<span>` with click handlers
- Images without dimensions
- Long lists without virtualization
- Inputs without labels; icon buttons without `aria-label`
- Hardcoded date/number formats
- Signal Orange `#EA580C` as normal-size text on white (3.56:1 — fails)
- White text on Signal Orange (fails)
- Status conveyed by color alone
- Any biometric/fingerprint/selfie UI — not part of this system
- Raw GPS coordinates displayed to any user
