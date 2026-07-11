# QA Report

## Independent Reviewer Pass (2026-07-11)

**Verdict: FAIL — one blocking issue found, builder's own PASS verdict does not hold.**

Method: re-served the actual repo (not trusting the builder's report) via
`python3 -m http.server` and drove a fresh headless Chrome instance directly
over CDP (chrome-devtools-mcp tool access was denied in this session, same
constraint the builder hit). Re-ran `.pipeline/qa/contrast-audit.js` and
`.pipeline/qa/upscale-audit.js` verbatim against index/classes/contact.html
at mobile (390×844), tablet (834×1112), and desktop (1440×1000). Note: the
first server start silently bound to a stale leftover `http.server` process
from a different, unrelated business already listening on the chosen port,
so the first audit run was against the wrong site entirely — caught by
checking the returned `<title>`, restarted on a verified-free port, and
re-ran clean. Independently verified all 5 image assets against the
business's live domain via SHA-256 (all byte-identical, confirming
BUILD_BRIEF's sourcing claims), and independently re-fetched
karateacademy.co.uk's own live pages (not just BUILD_BRIEF's summary of
them) to check the contact-email claim and several specific factual claims.

### Blocking issue: home hero text is materially dimmed by a decorative overlay the shared audit script cannot see
`assets/css/styles.css` line 244, `.hero::after`, is a decorative
`position: absolute; inset: 0` gradient (`tide` → `tide-deep`) at
`opacity: 0.5` on index.html's hero only (classes.html/contact.html's
`.page-hero` has no equivalent and is unaffected). Because `.hero .container`
and `.hero::after` are both positioned with `z-index: auto`, the
pseudo-element — generated after the real content in tree order — paints
**on top of** the hero copy (h1, lede, eyebrow) and the hero photo, per
standard CSS paint-order rules. `getComputedStyle(el).color` (what
`contrast-audit.js` reads, and what a dev checking DevTools' Computed panel
would see) still reports the authored color (e.g. `rgb(255,252,245)` for the
h1), so the script reports 0 violations here — but that is not the color a
visitor actually sees.

Verified by sampling actual rendered pixels from real screenshots (desktop
1440×1000 and mobile 390×844, both show the same effect):
| Element | Authored color (computed style) | Actual rendered pixel | Actual contrast vs bg (21,27,42) | Threshold | Result |
|---|---|---|---|---|---|
| `.hero .eyebrow` ("TRADITIONAL SHOTOKAN KARATE") | `--color-shu-bright` #e2703f | ≈ rgb(122,68,50) | **≈2.3:1** | 4.5:1 (small bold text) | **FAIL** |
| `.hero-copy p.lede` | `--color-sand-deep` #e8ddc7 | ≈ rgb(126,123,118) | **≈4.1:1** | 4.5:1 (18.4px normal weight) | **FAIL** |
| `.hero h1` | `--color-sand-white` #fffcf5 | ≈ rgb(138,139,143) | ≈5.0:1 | 3:1 (large bold) | Pass, but far below the ~16.6:1 the design assumed |

The eyebrow label is barely legible against the dark background in practice;
the lede paragraph genuinely fails AA. This is the exact "gradient/`::before`
layered background" risk AGENTS.md step 5 calls out for manual checking, but
in reverse — the gradient is layered *in front of* the text, not behind it,
which `contrast-audit.js` has no mechanism to detect (it only inspects
ancestor `background-color`/`background-image`, never a separately-painted
sibling/pseudo-element overlay). Recommend fixing this instance (remove the
overlay, drop its opacity further, restrict it to the photo half only, or
add `position/z-index` so it paints behind the text) **and** filing this as
a known blind spot against `.pipeline/qa/contrast-audit.js` per AGENTS.md's
instruction to fix shared-script bugs centrally.

### Advisory: BUILD_BRIEF.md Allowed Facts gap ("Shotokan")
The site states "Traditional Shotokan Karate" site-wide (title tag, meta
description, hero eyebrow/lede on index.html, footer tagline on all 3
pages), but BUILD_BRIEF.md's Allowed Facts table has no row for it, in
violation of AGENTS.md's "no factual claim... unless recorded in
BUILD_BRIEF.md with source evidence" rule. Independently re-checked: this
claim is true — the business's own live site states it explicitly ("Shotokan
is the style of karate that we practice, noted for its strength and
dynamism") on `enjoy_karate_4.html`, `karate_23.html`, `self_defence_2.html`,
and `what-they-say.html`. Not a fabrication, but a missing-citation
compliance gap — add the row before calling this fully compliant.

### Advisory: dropped qualifier on a competition result
classes.html's gradings section states "his brother Oscar took gold in
kata" for the English Karate Federation's 38th Annual Competition. The
source (`grading_results_34.html`) specifies this was "gold in the kata
category for people with additional needs" — a meaningful qualifier that
changes what the achievement represents. Not fabricated, but imprecise;
should be corrected to keep the category attribution.

### Confirmed: contact-email deviation is legitimate
Independently re-fetched `http://karateacademy.co.uk/contact_us_13.html`
live (plain HTTP, matching BUILD_BRIEF's note that HTTPS fails the TLS
handshake) and confirmed `info@karateacademy.co.uk` and
`admin@karateacademy.co.uk` do render live, site-wide, in the footer — the
builder's deviation from LEADS.md's logged `kaikokarate@aol.com` (which does
not appear anywhere on the current site or Companies House) is justified.
The aol.com address's status on Facebook remains genuinely unconfirmed
(JS-rendered, blocked automated fetch) — this was already flagged correctly
in BUILD_BRIEF.md/PIPELINE_STATUS.md and is not a new gap.

### Confirmed: images, upscale, broken-images, text-overflow, mobile nav
Re-running `.pipeline/qa/upscale-audit.js` fresh (not trusting the builder's
numbers) reproduced 0 violations, 0 broken images, 0 aspect mismatches
across all 3 pages × 3 viewports (index.html: 5/5 images clean at every
width; classes/contact: 0 `<img>` elements, as built). `contrast-audit.js`
reproduced 0 script-detected violations and the same 3 `needsManualCheck`
items per page (`.cta-banner` gradient text) — independently recomputed
their actual contrast (12.6:1 and 8.0:1 minimum) and confirmed they
genuinely pass, including `.cta-banner p`'s `--color-sand-deep` text, which
the builder's manual note didn't explicitly cover. All 5 image files are
SHA-256-identical to the live business-domain originals (no swap, no stock
substitution). Text-overflow re-checked against 12 box selectors with real
longest strings: 0 overflows at any viewport. Mobile nav correctly collapses
to a hamburger toggle at mobile/tablet widths and expands at desktop, with
no horizontal page overflow at any of the 3 widths on any page.

## Pages Checked
- index.html (Home)
- classes.html (Classes & Timetable)
- contact.html (Contact & Booking)

Method: local static server (`python3 -m http.server 4252`) rendered in headless Google Chrome, driven directly over the Chrome DevTools Protocol (a Node script using the built-in `WebSocket`/`fetch`, since the chrome-devtools-mcp tool was unavailable in this session). `.pipeline/qa/contrast-audit.js` and `.pipeline/qa/upscale-audit.js` were evaluated verbatim (unmodified) against the live rendered DOM via `Runtime.evaluate`, exactly as their own usage comments specify — not a static file read.

## Audit Results
| Check | Result | Evidence |
|---|---|---|
| Contrast audit | PASS (0 violations, all 3 pages) | index/classes/contact.html at 1440×1000 and 390×844: `violations: []` on every run. `needsManualCheck` flagged 3 items per page, all the `.cta-banner` gradient text — manually verified (see Manual Checks). |
| Upscale mobile (390×844) | PASS | index.html: 5 images checked, 0 violations, 0 brokenImages. classes.html / contact.html: 0 `<img>` elements present (text/icon-only pages by design), 0 violations. |
| Upscale tablet (834×1112) | PASS | Same as above, re-run at 834×1112: index.html 5/5 clean, classes/contact 0 images. |
| Upscale desktop (1440×1000) | PASS | Same as above, re-run at 1440×1000: index.html 5/5 clean, classes/contact 0 images. |
| Broken images | PASS | `brokenImages: []` on every page/viewport combination above. |
| Aspect mismatch advisory | none flagged | `aspectMismatches: []` on every run (all photos use `aspect-ratio` matching their native 1793:1200 ratio). |

## Manual Checks
| Check | Result | Notes |
|---|---|---|
| Text on photo | PASS | Only text-on-photo is the hero figcaption, which sits below the image (not overlaid), and the hero copy sits on the solid `--color-tide` background, not on the photo itself — no scrim needed. |
| Gradient/::before backgrounds | PASS (manually verified) | `.cta-banner` uses `linear-gradient(in oklch 120deg, var(--color-tide), var(--color-jade-deep) 130%)`. Computed contrast of `--color-sand-white` (#fffcf5) text against both gradient stops: 16.6:1 against #161c2b and 10.5:1 against #2c4238 — both far exceed WCAG AA 4.5:1. |
| Image/content match | PASS | Hero photo (Okinawa shrine, John Burke + Hokama Tetsuhiro Hanshi) matches its caption exactly; bunkai-practice photo matches the bunkai/oyo copy beside it; gallery photos captioned generically (no un-verifiable names attached to unidentified subjects). |
| Fabricated claims | PASS | Every factual claim on the site traces to a row in BUILD_BRIEF.md's Allowed Facts table with a live source URL; uncertain items (Ashburton, Facebook stats, the LEADS.md aol.com address) are listed in Do Not Claim and excluded from the copy. |
| Mobile layout | PASS | Rendered at 390×844 (iPhone-class) and 360×740 (narrower stress width): nav collapses to toggle menu, hero/gallery stack to one column, no horizontal scroll, no overlapping text. |
| Text-overflow (real content lengths) | PASS | Ran a `scrollWidth <= clientWidth` check at 360px width against `.info-card`, `.booking-item`, `.footer-grid`, `.location-card`, `.value-card`, `.testimonial-card`, `.price-card`, and `table.schedule` — the actual longest real strings in use (full postal address "Karate Academy, King Street Studios, King Street, Newton Abbot, Devon, TQ12 2LG", full email address, full booking URLs `https://apps.apple.com/gb/app/karate-academy/id1542741931` and `https://karateacademy.goretrieve.com`). 0 of 25 checked boxes overflowed on any page. |

## Blocking Issues
| Issue | Evidence | Required fix |
|---|---|---|
| Hero/gallery image files were swapped during asset copy (Okinawa shrine photo and Devon beach photo had each other's filenames) | First screenshot pass showed the beach photo under the Okinawa caption | Re-copied the two source files to the correct target filenames and re-verified with a fresh screenshot before sign-off — see Fixed Verification |

## Advisory Issues
- None outstanding.

## Fixed Verification
| Issue | Fix | Recheck result |
|---|---|---|
| okinawa-training.jpg / beach-black-belts.jpg swapped | Re-copied `mi207_l.jpg` → `okinawa-training.jpg` and `mi815_l.jpg` → `beach-black-belts.jpg` (previously reversed) | Re-screenshotted index.html hero at 1440×1000: shrine/Hanshi photo now correctly appears under its matching caption; re-ran upscale audit on index.html post-fix, still 0 violations / 0 broken images |

## Verdict (builder's original pass, superseded — see Independent Reviewer Pass above)
PASS (builder's self-check; not confirmed by independent review)

## Final Verdict (superseded — see Fix Verification Pass below)
**FAIL** — blocking: home hero text (eyebrow + lede) is materially dimmed
below WCAG AA by the `.hero::after` overlay's paint order (see Independent
Reviewer Pass). Not deployable as-is. Advisory: BUILD_BRIEF.md missing a
sourced row for the "Shotokan" claim; classes.html drops the "additional
needs" category qualifier on Oscar Magor's gold medal.

## Fix Verification Pass (2026-07-11)

All three issues from the Independent Reviewer Pass were fixed and
re-verified.

### Blocking: `.hero::after` overlay paint order
**Fix:** `assets/css/styles.css` — added `z-index: 1;` to `.hero .container`
(line ~253, next to its existing `position: relative`). `.hero::after` keeps
`z-index: auto` (effectively stacked at the base layer), so the hero copy and
media now paint in a positioned stacking context above the decorative
overlay instead of the overlay painting on top of the text in DOM order.

**Verification method:** Rejected re-checking via `getComputedStyle()` alone
(that's exactly what let this bug through originally). Instead, launched
actual headless Chrome (`google-chrome --headless=new --remote-debugging-port`)
against the real repo served over `python3 -m http.server`, drove it directly
over the Chrome DevTools Protocol with a Node script (`Page.captureScreenshot`),
and sampled true rendered pixel colors from the PNG output with Python/PIL —
a dense grid of sample points per text element, taking the max-luminance
pixel per element (the actual glyph color, light-on-dark design) against a
clean background sample point elsewhere in `.hero` with no text. Computed
WCAG relative-luminance contrast from the sampled RGB pairs, not from
authored/computed CSS values.

| Element | Viewport | Rendered pixel (post-fix) | Contrast vs bg | Threshold | Result |
|---|---|---|---|---|---|
| `.hero .eyebrow` | mobile 390×1300 | rgb(226,112,63) — matches authored `--color-shu-bright` exactly | 5.51:1 | 4.5:1 (small bold) | **PASS** |
| `.hero .eyebrow` | desktop 1440×1000 | rgb(226,112,63) | 5.39:1 | 4.5:1 | **PASS** |
| `.hero-copy p.lede` | mobile 390×1300 | rgb(226,215,194) — matches authored `--color-sand-deep` | 12.25:1 | 4.5:1 (normal weight) | **PASS** |
| `.hero-copy p.lede` | desktop 1440×1000 | rgb(206,198,179) | 10.06:1 | 4.5:1 | **PASS** |
| `.hero h1` | mobile 390×1300 | rgb(255,252,245) — matches authored `--color-sand-white` | 17.04:1 | 3:1 (large bold) | **PASS** |
| `.hero h1` | desktop 1440×1000 | rgb(255,252,245) | 16.69:1 | 3:1 | **PASS** |

The sampled pixel colors now match the authored CSS custom-property values
exactly (previously the review found rendered colors visibly shifted toward
the dark overlay gradient, e.g. eyebrow rendering as ≈rgb(122,68,50) instead
of its authored #e2703f) — confirming the overlay no longer paints over the
text at any of the checked viewports. A visual screenshot crop of the
desktop hero was also inspected directly and shows crisp, undimmed text
against the dark background.

Also re-ran `.pipeline/qa/contrast-audit.js` and `.pipeline/qa/upscale-audit.js`
against the fixed repo at mobile (390×844), tablet (834×1112), and desktop
(1440×1000) on all 3 pages: 0 violations, 0 broken images, 0 aspect
mismatches, same 3 `needsManualCheck` `.cta-banner` gradient-text items as
before (previously manually confirmed to pass at 12.6:1/8.0:1+).

**Known blind spot filed:** `.pipeline/qa/contrast-audit.js` only inspects
`getComputedStyle()` and ancestor `background-color`/`background-image`; it
has no mechanism to detect a separately-painted sibling/pseudo-element that
visually overlays text via paint order/stacking context while leaving
computed style untouched. This class of bug will recur silently on any
future build that layers a decorative `::before`/`::after` overlay without
explicit z-index below the text. Flagging here per AGENTS.md's instruction
to note shared-script bugs; the script itself was not modified this pass
(no safe general fix identified — would require rendering-based pixel
sampling, which the script does not currently do).

### Advisory: BUILD_BRIEF.md "Shotokan" Allowed Facts gap
**Fix:** Added an Allowed Facts row for "Traditional Shotokan Karate" citing
`enjoy_karate_4.html`, `karate_23.html`, `self_defence_2.html`, and
`what-they-say.html` on karateacademy.co.uk (all state the claim directly),
noting it's used site-wide (title tag, meta description, home hero
eyebrow/lede, footer tagline on all 3 pages). **Verified:** row present in
BUILD_BRIEF.md's Allowed Facts table.

### Advisory: dropped competition-category qualifier
**Fix:** `classes.html`'s gradings testimonial now reads "...his brother
Oscar took gold in the kata category for people with additional needs"
(previously unqualified "gold in kata"), matching `grading_results_34.html`'s
exact wording. **Verified:** grep of `classes.html` confirms the corrected
sentence is present and the old unqualified phrasing no longer appears.

## Final Verdict
**PASS** — all 3 issues from the Independent Reviewer Pass fixed and
independently re-verified with real rendered-pixel sampling (not just
computed-style contrast). Deployable.
