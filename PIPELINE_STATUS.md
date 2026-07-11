# Pipeline Status

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: **Fixed, re-verified, and deployed. Outreach draft skipped — see below.**
- Last trusted commit: `7b73f6a` "Fix hero contrast overlay paint order, source Shotokan claim, correct gradings qualifier" on this repo's `main` branch, pushed to `Plainset/karate-academy-newton-abbot-demo`.
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
- Deploy: **done.** Created public GitHub repo `Plainset/karate-academy-newton-abbot-demo`,
  pushed `main`, enabled Pages (`source[branch]=main`, `source[path]=/`).
  Build status polled until `built`; confirmed live with `curl` — index.html,
  classes.html, contact.html all return HTTP 200, `<title>` matches expected
  content.
- Deploy URL: **https://plainset.github.io/karate-academy-newton-abbot-demo/**
- Outreach state: **not drafted.** Before composing, attempted to confirm the
  active signed-in Gmail account per the required "check the account
  switcher" step. Both available means of doing that in this session were
  permission-denied: the browser tool (`chrome-devtools new_page`, needed to
  visually inspect the account switcher) and the Gmail MCP connector's own
  read tool (`search_threads`, which could have at least confirmed the
  authenticated mailbox indirectly). With neither available, there was no way
  to confirm with high confidence that `vdvalkproductions@gmail.com` (and not
  `alex.vdv0012@gmail.com` or `openclawsandbox96@gmail.com`) is the active
  account, so per the explicit instruction, Gmail was not touched at all — no
  draft was composed. This needs a human to either grant the browser-tool
  permission or confirm the active account another way before outreach can be
  drafted for this business.
- Next exact action: a human (or a session with chrome-devtools/browser
  permission granted) needs to confirm the active Gmail account, then draft
  the outreach email per AGENTS.md step 7 / the exact template, to
  `info@karateacademy.co.uk`, subject "Website idea for Karate Academy Ltd",
  hook: both karateacademy.co.uk and karatenewtonabbot.com fail the HTTPS
  handshake outright (browser "connection not private" warning) despite the
  underlying site being a genuinely rich, actively-updated multi-page site.
  Demo link: https://plainset.github.io/karate-academy-newton-abbot-demo/
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
