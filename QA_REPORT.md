# QA Report

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

## Verdict
PASS
