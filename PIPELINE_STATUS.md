# Pipeline Status

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: **All 3 review issues fixed and independently re-verified — proceeding to deploy.**
- Last trusted commit: fixes applied on top of the initial commit on this repo's `main` branch (see `git log`); not yet committed as of this status update — see next action.
- Fixes applied (2026-07-11), all re-verified — see `QA_REPORT.md`'s "Fix Verification Pass" section for full evidence:
  1. `assets/css/styles.css` `.hero .container` — added `z-index: 1;` so the
     hero copy/media paint above `.hero::after`'s decorative overlay instead
     of underneath it. Verified with real rendered-pixel sampling (headless
     Chrome over CDP + Python/PIL), not just `getComputedStyle()`: eyebrow
     5.4-5.5:1, lede 10.1-12.3:1, h1 16.7-17.0:1 at both mobile (390×1300)
     and desktop (1440×1000) — all clear WCAG AA. Rendered pixel colors now
     match authored CSS values exactly (previously shifted dark toward the
     overlay gradient).
  2. `BUILD_BRIEF.md` — added an Allowed Facts row for "Traditional Shotokan
     Karate", sourced to karateacademy.co.uk's `enjoy_karate_4.html`,
     `karate_23.html`, `self_defence_2.html`, `what-they-say.html`.
  3. `classes.html` — gradings testimonial corrected to "gold in the kata
     category for people with additional needs" (was unqualified "gold in
     kata"), matching `grading_results_34.html`.
  - Known blind spot filed (not fixed, no safe general fix identified):
    `.pipeline/qa/contrast-audit.js` cannot detect a decorative
    pseudo-element that overlays text via paint order/stacking context while
    leaving `getComputedStyle()` untouched — it only inspects computed style
    and ancestor backgrounds. Future builds using a decorative `::before`/
    `::after` overlay must set explicit z-index so it stacks behind text.
- Next exact action, in order:
  1. Commit the fixes.
  2. Re-run `.pipeline/qa/contrast-audit.js` and `.pipeline/qa/upscale-audit.js`
     (done — see QA_REPORT.md, clean pass, 0 violations/broken images).
  3. Deploy: create public GitHub repo `Plainset/karate-academy-newton-abbot-demo`,
     push, enable Pages (source[branch]=main, source[path]=/), confirm live URL loads.
  4. Draft outreach in `vdvalkproductions@gmail.com` only, per AGENTS.md step 7 — draft only, never send.
- Deploy URL: pending this session's deploy step — see final report.
- Outreach state: pending this session's draft step — see final report.
- Contact-email deviation: **independently confirmed safe to proceed on, twice
  over** (builder + independent reviewer). Re-fetched
  `http://karateacademy.co.uk/contact_us_13.html` live and confirmed
  `info@karateacademy.co.uk` (and `admin@karateacademy.co.uk`) render
  site-wide in the footer of the business's actual current site. LEADS.md's
  logged `kaikokarate@aol.com` does not appear anywhere on the current
  domain or Companies House; Facebook could not be re-scraped
  (JS-rendered/blocked) so that address's current status there remains
  genuinely unconfirmed either way — an accepted, correctly-flagged gap, not
  a new one. **Deviation from LEADS.md:** outreach uses
  `info@karateacademy.co.uk` as the To address, not the LEADS.md-logged
  `kaikokarate@aol.com`, per this confirmed finding.
