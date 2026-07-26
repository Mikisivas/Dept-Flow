# Color and type system

Departmental colors: **orange, white, black** — taken from the SAMACOSS crest
(Student Association of Mathematics, Computer Science & Statistics). This file turns
those three into a working, accessible system.

## The palette is derived from the logo

The crest's orange is a **golden orange, hue ≈32°** — noticeably warmer and lighter
than a generic "web orange." The whole UI scale is generated from that hue so the
interface and the logo read as one system.

> **Confirm the exact value before launch.** `#F0952B` is a close visual read of the
> supplied crest, not a sample from the source file. Open the original in any editor,
> eyedrop the shield fill, and if it differs, update `--brand` here and regenerate
> the scale — the ratios below shift with it.

**The logo already tells us the correct text treatment.** "SAMACOSS" is set in
**black on orange**, not white. Follow that: orange is a *fill* that carries black
text. This is not a compromise — it measures better than the alternative.

## Verified contrast ratios

WCAG 2.1 relative-luminance formula. AA requires **4.5:1** for normal text, **3:1**
for large text (≥18.66px bold / ≥24px) and UI component boundaries.

| Pair | Ratio | Verdict |
|---|---|---|
| **Black `#0A0A0A` on Brand Orange** | **8.52:1** | ✓ AAA — *the primary pattern* |
| Brand Orange `#F0952B` on white | **2.32:1** | ✗ fails — never as text |
| White on Brand Orange | **2.32:1** | ✗ fails — never do this |
| Black on Hover `#D67A0F` | **6.27:1** | ✓ AA |
| Black on Pressed `#BF6D0D` | **5.10:1** | ✓ AA |
| Orange Text `#A75F0C` on white | **4.90:1** | ✓ AA — the only orange safe as text |
| Ink `#0A0A0A` on white | **19.80:1** | ✓ AAA |
| Slate `#525252` on white | **7.81:1** | ✓ AAA — secondary text |
| Muted `#737373` on white | **4.74:1** | ✓ AA — tertiary text, captions |
| White on Green `#15803D` | **5.02:1** | ✓ AA |
| White on Red `#B91C1C` | **6.47:1** | ✓ AA |
| White on Blue `#1D4ED8` | **6.70:1** | ✓ AA |
| Brand Orange on Ink (dark mode) | **8.52:1** | ✓ AAA — works unchanged on dark |

**The trap:** the instinct is a white-text-on-orange button. At **2.32:1** that is
one of the worst contrast failures possible and would be caught immediately. Orange
surfaces carry **black** text. If orange must be *text* on white, it has to darken
all the way to `#A75F0C`.

## Tokens

```css
:root {
  /* Brand — hue 32°, generated from the SAMACOSS crest */
  --brand:            #F0952B;  /* crest orange — fills, motif, primary button. BLACK text. */
  --brand-hover:      #D67A0F;  /* button hover (black text: 6.27:1) */
  --brand-pressed:    #BF6D0D;  /* button pressed (black text: 5.10:1) */
  --brand-text:       #A75F0C;  /* the ONLY orange usable as text on white (4.90:1) */
  --brand-tint:       #FDF3E7;  /* barely-orange surface for grouped panels */
  --brand-tint-2:     #FCE7CF;  /* selected rows, subtle emphasis */

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
    --brand:          #F0952B;  /* crest orange is already 8.52:1 on ink — keep it */
    --brand-text:     #F2A040;  /* lighten only the text variant for dark surfaces */
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

## Logo usage and required assets

The SAMACOSS crest is the identity mark: an orange shield holding a monitor-and-tower
glyph, the SAMACOSS wordmark, and a book, wrapped in two white ribbon banners
("Student Association of Mathematics Computer Science & Statistics" / "Towards
Advanced Technology").

**It is a detailed crest, and detail does not survive small sizes.** The ribbon
outlines are hairlines and the banner text is tiny; below roughly 200px it turns to
mud, and at favicon size (16–32px) it is unreadable noise. So the crest is used at
size, and a simplified mark is used everywhere small.

| Asset | Source | Where used |
|---|---|---|
| **Full crest** | supplied logo | login/landing screen, printed reports, About |
| **App mark** | shield silhouette + monitor glyph only — **no ribbons, no banner text** | app header, favicon, PWA icon, loading screen |
| **Monochrome mark** | app mark, single-color | dark mode, watermarks, anywhere over orange |

Required files (all derived from the one crest):
- `favicon.ico` — 32×32, app mark only
- `icon-192.png`, `icon-512.png` — PWA/home-screen, app mark on the crest orange
- `apple-touch-icon.png` — 180×180
- `logo-full.svg` — the crest, for the login screen
- `logo-mark.svg` — the simplified app mark

**Ask for the source file.** The crest as supplied is a raster on an opaque white
background. Get the original **SVG or transparent PNG** from whoever produced it —
a white box behind the logo will be visible against `--brand-tint` panels and will
look broken in dark mode. If only a raster exists, the mark should be redrawn as SVG
rather than scaled up.

**Never** recolor the crest, stretch it, place the full crest on an orange fill (the
shield disappears), or add effects to it.

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
