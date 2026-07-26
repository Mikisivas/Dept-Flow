# Screen and component patterns

Per-role layouts and shared components. Product logic and state definitions:
`docs/system-operation-and-logic.md`.

## Shared components

### CheckpointStrip — the signature component
Renders a session (or a run of sessions) as checkpoint pairs.

```
Single session:   ▮▮ 1.0     ▮▯ 0.5     ▯▯ 0     ⌐⌐ provisional (dashed)
Semester strip:   ▮▮ ▮▯ ▮▮ ▮▮ ▯▯ ▮▮ ▮▯ ▮▮ …
```

- Filled cell = accepted checkpoint (Brand Orange `#F0952B`, the crest orange)
- Hollow cell = missed checkpoint (1px `--line` outline)
- Dashed cell = provisional, not yet counted
- Single-checkpoint session (lecturer issued one token) renders as one wide cell, so
  it is visibly not a pair — never fake a second cell
- Manual/paper batch carries a small corner mark; hovering/tapping reveals
  "Recorded from paper register"
- Each cell needs an accessible label (`aria-label="Week 4, both checkpoints
  captured"`) — the motif must not be the only carrier of meaning

### StatusBadge
Icon + label + color, never color alone.

| State | Color | Label |
|---|---|---|
| Confirmed | `--ok` | "Counted" |
| Provisional | dashed neutral | "Not yet counted" |
| Pending verification | `--info` | "Checking payment…" |
| Locked | `--danger` filled | "Attendance locked" |
| At risk | `--danger` outlined | "At risk" |

### AttendanceMeter
The 75% threshold is the whole point — the meter must show the line, not just the
value. A bare percentage with no threshold marker is a failed design here.

- Horizontal bar, orange fill, a hard tick at 75% with a label
- Below the number: the actionable sentence — "You need 4 more full sessions to
  reach 75%"
- If any sessions are provisional, show them as a dashed segment beyond the solid
  fill, so the student sees what they'd gain by clearing
- `tabular-nums` on the percentage

## Student

**Dashboard (the most-used screen in the system).** Priority order top to bottom:
1. **Compliance state** — if locked or provisional, this is the first thing on the
   screen with the fix action attached. Never bury it under a greeting.
2. **AttendanceMeter** per course, with the 75% line.
3. **CheckpointStrip** for the semester.
4. Risk nudge, if any — worded by pattern: trending 0s → "You've missed 3 full
   classes"; trending 0.5s → "You're catching only one checkpoint — try to stay
   till the end."

**Token entry.** The highest-frequency, most time-pressured interaction in the
product. It happens in a noisy hall with a 3–5 minute window.
- Big numeric input, `inputmode="numeric"`, `autocomplete="one-time-code"`
- **Never block paste**
- Auto-advance between digit boxes, and allow paste of the whole code
- Show the countdown to token expiry
- One clear result state: accepted (with which checkpoint it was) or rejected
  **with the reason** — "You're outside the lecture hall," "This code expired,"
  "Your account is locked." A generic failure here will generate disputes.
- Submitting must work on a bad connection: optimistic local state + retry, with an
  honest "not yet confirmed" indicator until the server acknowledges

**Payment.** Card and Pay with Transfer only. Show the dues amount, the deadline,
and — critically — what clearing unlocks ("This will count your 12 waiting
sessions"). After paying, never leave the student on an ambiguous screen: show
"Checking payment…" (`--info`) until the webhook confirms.

## Lecturer

**Session control.** One primary action at a time, large tap targets — this is
operated while standing in front of a class.
- Start Session → then a single prominent **"Generate checkpoint code"** button
- Display the generated 4-digit code **very large** (it gets written on a
  whiteboard) with the expiry countdown
- Show live count of submissions as they arrive
- Clearly indicate which checkpoint this is (1st or 2nd) and that a second one is
  expected before the session closes
- End Session confirms if only one checkpoint was issued ("This session will be
  scored present/absent, not out of two checkpoints — continue?")

**Manual/paper batch.** Reached only from a closed session. Two-column entry
mirroring the paper sheet, mandatory justification note, and an explicit warning
that the entry is flagged and reviewable. This screen should feel heavier than the
normal flow — it bypasses the anti-proxy checks.

**Schedule.** Create makeup / reschedule / cancel for own courses only. Cancelling
must state its consequence: "This session won't count toward anyone's total."

## HOD

Academic governance, individual students visible.
- **Risk list** — students trending below 75%, sorted by severity, each row showing
  the CheckpointStrip so the pattern is legible at a glance
- **Grace period control** — the highest-consequence control in the app. Show
  exactly who it affects and how many, require a reason, confirm before applying,
  and state the expiry date in plain language. Log-writing action.
- **Waivers, disputes, final eligibility list** — the eligibility list is an
  authorization action, not an export; treat the confirm step seriously.
- HOD does **not** see dues configuration, geo-fence coordinates, or the whitelist.

## Admin

Operations and infrastructure. Aggregate signals only — **no individual student
risk alerts** (that's HOD's scope; showing it here breaks the separation of duties).
- Whitelist upload with a clear preview/diff before committing
- Registration disputes: revoke + reclaim, with reason, audit-logged
- Deactivation (Expelled / Withdrawn / Graduated / Other) — soft delete, confirm,
  reason required
- Level rollover — a single bulk action with a strong confirmation showing exactly
  how many students move and to what level; irreversible in practice, so the
  confirm must be explicit
- System health: payment reconciliation failures, GPS-rejection rate spikes,
  aggregate compliance percentages

## Layout rules

- **Mobile-first, always.** Students only ever use phones. Design the phone layout
  first; desktop is the HOD/admin secondary case.
- Tables collapse to stacked cards below `md` — never horizontal-scroll a data table
  as the primary mobile experience.
- Sticky primary action on mobile (pay, submit code) so it survives a long scroll.
- Bottom-sheet pattern for token entry and confirmations on mobile —
  `overscroll-behavior: contain`.
- Respect `env(safe-area-inset-*)`.
