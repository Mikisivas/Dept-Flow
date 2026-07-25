# Color and type system

Departmental colors: **orange, white, black.** This file turns those three into a
working, accessible system.

## Verified contrast ratios

Computed with the WCAG 2.1 relative-luminance formula. AA requires **4.5:1** for
normal text, **3:1** for large text (≥18.66px bold / ≥24px) and for UI component
boundaries.

| Pair | Ratio | Verdict |
|---|---|---|
| Signal Orange `#EA580C` on white | **3.56:1** | ✗ fails normal text — large text / UI only |
| Black `#0A0A0A` on Signal Orange | **5.56:1** | ✓ AA — *this is how to use brand orange* |
| White on Deep Orange `#C2410C` | **5.18:1** | ✓ AA — primary button |
| Deep Orange `#C2410C` on white | **5.18:1** | ✓ AA — orange text |
| Ink `#0A0A0A` on white | **19.80:1** | ✓ AAA |
| Slate `#525252` on white | **7.81:1** | ✓ AAA — secondary text |
| Muted `#737373` on white | **4.74:1** | ✓ AA — tertiary text, captions |
| White on Green `#15803D` | **5.02:1** | ✓ AA |
| White on Red `#B91C1C` | **6.47:1** | ✓ AA |
| White on Blue `#1D4ED8` | **6.70:1** | ✓ AA |
| Bright Orange `#FB923C` on Ink | **8.75:1** | ✓ AAA — dark mode brand |

**The trap:** the natural instinct is to make the brand orange the button color with
white text. That combination is ~2.6:1 and badly fails. Use Deep Orange `#C2410C`
for any orange surface that carries white text.

## Tokens

```css
:root {
  /* Brand */
  --brand:            #EA580C;  /* Signal Orange — fills, motif, charts. BLACK text on it. */
  --brand-strong:     #C2410C;  /* Deep Orange — orange text on white; white-text buttons */
  --brand-deep:       #9A3412;  /* hover/pressed for brand-strong */
  --brand-tint:       #FFF7ED;  /* barely-orange surface for grouped panels */
  --brand-tint-2:     #FFEDD5;  /* selected rows, subtle emphasis */

  /* Neutrals */
  --ink:              #0A0A0A;  /* primary text */
  --slate:            #525252;  /* secondary text */
  --muted:            #737373;  /* tertiary text, captions, timestamps */
  --line:             #E5E5E5;  /* borders, dividers */
  --surface:          #FFFFFF;
  --surface-sunken:   #FAFAFA;  /* table stripes, page background */

  /* Status — deliberately clear of the brand */
  --ok:               #15803D;  /* confirmed, cleared, paid */
  --ok-tint:          #DCFCE7;
  --info:             #1D4ED8;  /* pending verification */
  --info-tint:        #DBEAFE;
  --danger:           #B91C1C;  /* locked */
  --danger-tint:      #FEE2E2;
  /* provisional has no colour of its own — see below */
}

@media (prefers-color-scheme: dark) {
  :root {
    --brand:          #FB923C;  /* lighter orange reads on dark; 8.75:1 on ink */
    --brand-strong:   #FDBA74;
    --ink:            #FAFAFA;
    --slate:          #A3A3A3;
    --muted:          #737373;
    --line:           #262626;
    --surface:        #0A0A0A;
    --surface-sunken: #171717;
    --ok:             #4ADE80;
    --info:           #60A5FA;
    --danger:         #F87171;
  }
}
```

Set `color-scheme: dark` on `<html>` in dark mode and match
`<meta name="theme-color">` to the surface color.

## Why provisional has no color

Provisional attendance is not a warning and not an error — it is a record that
exists but does not yet count. Giving it a color implies a judgment. Instead:

- **dashed 1px border**, `--muted` text, no fill
- the checkpoint cells render as **hollow with a dashed edge** rather than solid orange
- always paired with the sentence that explains it and the action that fixes it

Once the student clears, the same cells fill solid orange and the border goes solid.
That transition — hollow to filled — is the most important visual moment in the
product, because it is the payoff for paying dues. Design it deliberately (a brief
fill animation is justified here; honor `prefers-reduced-motion`).

## Using orange without drowning in it

Orange is loud. A screen where every element is orange loses all hierarchy and stops
looking institutional. Budget it:

- **~10% of the screen maximum.** Orange marks the primary action, the checkpoint
  motif, and the active nav item. Nothing else.
- Structure comes from **black type on white with thin gray rules**, not from orange
  panels.
- Never use orange for large background washes behind text.
- Never use orange for a status that is not the brand's primary action.

## Typography

A single well-set family beats a mismatched pairing here — this is an instrument,
and legibility on cheap Android screens outranks personality.

- **Body / UI:** `Inter` (or the system stack) — 16px base, never below 14px for
  anything a student must read. Weights 400/500/600.
- **Data / numbers:** the same family with
  `font-variant-numeric: tabular-nums` — mandatory anywhere numbers stack
  (attendance %, amounts, matric numbers, session counts) so columns align and
  digits don't jitter as values update.
- **Display:** the same family at 600/700 and tight tracking. If you want one
  characterful accent face, confine it to the app wordmark only — not headings.

Load one variable font subset with `font-display: swap` and preload it. Every extra
font file is real money on a student's data bundle.

Scale: `12 / 14 / 16 / 20 / 24 / 32`. Use `text-wrap: balance` on headings.

## Copy tone

Plain, second person, active voice, no apology. The system enforces rules and should
say so without sounding punitive.

- "Your dues aren't cleared yet. 12 sessions are waiting to be counted." — not
  "Payment compliance violation detected."
- "Pay dues" — not "Proceed to payment portal."
- "Attendance closed for this session." — not "Oops! Something went wrong."
- Errors always state the fix. Empty states always state the next action.
