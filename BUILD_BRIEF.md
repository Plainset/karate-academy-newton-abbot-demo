# Build Brief

## Contact
- Email: info@karateacademy.co.uk (also seen as admin@karateacademy.co.uk, same domain/owner)
- Email source URL: http://karateacademy.co.uk/contact_us_13.html (footer of every page on the site, e.g. also http://karateacademy.co.uk/karate_timetable_10.html, http://karateacademy.co.uk/book-classes.html)
- Rechecked date: 2026-07-11 (fetched live via curl; both info@karateacademy.co.uk and admin@karateacademy.co.uk render on the current site)
- **Discrepancy vs LEADS.md:** LEADS.md logs `kaikokarate@aol.com` for this business. That address does not appear anywhere on the business's own domain (checked homepage, contact page, timetable, kids/adults/FAQ pages) or on the company's Companies House record. It may be an older personal/Facebook-listed address not currently displayed on the site. Per AGENTS.md step 2 ("confirm a usable email exists on the business's own site/contact page"), the demo uses `info@karateacademy.co.uk`, which is live, current, and repeated across the whole site as of the rebuild date. Facebook itself (facebook.com/theKarateAcademy) could not be independently re-scraped (JS-rendered, blocked automated fetch), so the aol.com address could not be confirmed or denied there.
- Phone: 01626 360999 (matches LEADS.md exactly; confirmed live on every page of karateacademy.co.uk)
- Address: Karate Academy, King Street Studios, King Street, Newton Abbot, Devon, TQ12 2LG (matches LEADS.md and Companies House registered office exactly)

## Business State Check
- Status: **still open at logged address**, actively trading
- Checked sources:
  - Companies House (find-and-update.company-information.service.gov.uk/company/06128348): "KARATE ACADEMY LTD", status **Active**, incorporated 26 Feb 2007, registered office "King Street Studios, King Street, Newton Abbot, England, TQ12 2LG" — exact match to LEADS.md.
  - Own site http://karateacademy.co.uk (plain HTTP; HTTPS fails TLS handshake exactly as LEADS.md describes, confirmed again via `curl -vIk https://karateacademy.co.uk`, LibreSSL "tlsv1 alert internal error") — timetable page states "Times correct as of 2nd July 2026" (9 days before this rebuild).
  - Grading results page shows dated gradings on 7 March 2026 and 20 June 2026, and a reference to the "English Karate Federation's 38th Annual Competition" — recent, specific, and internally consistent with named students appearing elsewhere on the site.
  - Booking page links to a live App Store listing (id1542741931) and Google Play listing (com.goretrieve.karateacademy), both under the name "Karate Academy".
- Notes: Own domains (karateacademy.co.uk, karatenewtonabbot.com) both fail TLS handshake outright over HTTPS, exactly as logged in LEADS.md — real prospects hit a "connection not private" browser warning. Plain HTTP still serves a full, actively-maintained multi-page site.
- Build decision: **proceed**

## Multi-location check
Genuinely multi-location, not just "a venue": classes run at **Newton Abbot** (King Street Studios — head office, matted centre, 7 days/week) and **Torquay** (St Martin's Church Hall, Barton Hill Road — Tuesdays 6pm only). The site's own FAQ confirms only these two towns currently ("Our classes are held across the towns of Newton Abbot, and Torquay"). Older marketing copy in the site's header/footer strapline also names "Ashburton", but no Ashburton class appears on the live timetable — treated as stale copy, not claimed (see Do Not Claim).
Per AGENTS.md step 3, this build covers both locations honestly (a Torquay entry alongside Newton Abbot on the timetable page and in the location cards) rather than silently dropping Torquay, and the demo is scoped/branded explicitly as "Karate Academy — Newton Abbot & Torquay" rather than implying a single-venue business.

## Page Plan
- Scope: 3-page default
- Pages: index.html (Home), classes.html (Classes & Timetable), contact.html (Contact & Booking)
- Reason for any extra page: none needed — all verified content (age groups, timetable, credentials, gradings, membership, FAQ) fit comfortably into the 3-page default without overcrowding.

## Pitch Hook
- Verified observation: Both of the business's own domains (karateacademy.co.uk and karatenewtonabbot.com) fail the HTTPS handshake outright, so any visitor who reaches the site via a search result or a bookmarked https:// link is met with a browser security warning ("connection not private") before they ever see the page — despite the underlying site itself being a genuinely rich, actively-updated, multi-page site with real class content, gradings, and testimonials.
- Source URL: `curl -vI https://karateacademy.co.uk` and `curl -vI https://karatenewtonabbot.com` (both return LibreSSL/TLS handshake failure); http://karateacademy.co.uk (plain HTTP) loads the full real site.

## Allowed Facts
| Fact | Source URL | Used where |
|---|---|---|
| Karate Academy Ltd, Active, incorporated 26 Feb 2007 | find-and-update.company-information.service.gov.uk/company/06128348 | Home quick-facts ("2007"), footer |
| Registered office / head office: King Street Studios, King Street, Newton Abbot, Devon TQ12 2LG | Companies House (above) + http://karateacademy.co.uk/contact_us_13.html | All pages, contact/footer |
| Phone 01626 360999 | http://karateacademy.co.uk/contact_us_13.html (repeated site-wide) | All pages |
| Email info@karateacademy.co.uk (also admin@karateacademy.co.uk) | http://karateacademy.co.uk/contact_us_13.html | All pages |
| Torquay class: St Martin's Church Hall, Barton Hill Road, Tuesdays 6pm | http://karateacademy.co.uk/karate_timetable_10.html and karate_faqs_6.html | classes.html locations + timetable |
| Newton Abbot 7-day timetable (Mon–Sun class list) | http://karateacademy.co.uk/karate_timetable_10.html ("Times correct as of 2nd July 2026") | classes.html timetable |
| Chief instructor John Burke renshi, teaching since 1996, trained repeatedly in Japan/Okinawa, wrote articles for Traditional Karate magazine | http://karateacademy.co.uk/teaching_karate_12.html, enjoy_karate_4.html, self_defence_2.html | Home bio section |
| John Burke: Nanadan (7th Dan) awarded 18/05/2024 | http://karateacademy.co.uk/karate_black_belts_21.html | Home badge row |
| Licensed by Eikoku Karate-do Keikokai; recognised by Traditional Karate Study Group International; Black Belt gradings authorised by Anthony Blades hanshi | http://karateacademy.co.uk/karate_faqs_6.html and karate_black_belts_21.html | Home bio, classes.html credentials |
| Guest instructor Hanshi Patrick McCarthy, 9th Dan | http://karateacademy.co.uk/karate_black_belts_21.html (photo caption) | Home value card |
| Instructors: Enhanced DBS (CRB) check; insured for member-to-member training, public liability, professional indemnity; minimum 500 hours' training | http://karateacademy.co.uk/karate_faqs_6.html | Home + classes.html credentials |
| Little Warriors (4–7), Juniors (from age 5), Adults classes; free trial lessons; no contracts | http://karateacademy.co.uk/karate_for_kids_7.html, karate_for_adults_8.html, karate_timetable_10.html | classes.html age groups |
| Membership £45/month, no contract, free initial consultation | http://karateacademy.co.uk/book-classes.html | classes.html pricing |
| Booking app: App Store id1542741931; Google Play com.goretrieve.karateacademy; member portal karateacademy.goretrieve.com | http://karateacademy.co.uk/book-classes.html | contact.html booking list |
| Gradings 7 March 2026 (Sandan/Nidan/Shodan) and 20 June 2026 (Little Warriors stripes); English Karate Federation 38th Annual Competition results (Joe Magor silver/bronze, Oscar Magor gold) | http://karateacademy.co.uk/grading_results_34.html and karate_black_belts_21.html | classes.html recent gradings |
| Testimonials: Terry O'Neill (7th Dan, Merseyside), Andy (visiting club), Dave Cook sensei (shodan, Cornwall) | http://karateacademy.co.uk/what-they-say.html | Home testimonials |
| Uniform (gi) costs under £25; no uniform needed to start; no obligation to compete; typical class structure | http://karateacademy.co.uk/karate_faqs_6.html | classes.html FAQ |

## Do Not Claim
| Claim or uncertainty | Reason |
|---|---|
| kaikokarate@aol.com as the contact email | Not found on the business's own current site or Companies House; could not verify on Facebook (blocked automated fetch). Used the live on-site email instead — see Contact section. |
| Ashburton as a current class location | Named in old header/footer strapline copy ("Newton Abbot, Torquay, and Ashburton") but the site's own FAQ and timetable list only Newton Abbot and Torquay, and no Ashburton schedule exists anywhere on the site. Treated as stale copy. |
| Facebook like/check-in counts (~795 likes / 2,971 check-ins per LEADS.md) | Facebook page could not be re-scraped in this session (JS-rendered, request blocked); not independently re-verified today, so left out of the demo entirely rather than repeated on trust. |
| Specific per-class adult/child fee breakdown | Only the £45/month membership figure is quoted on the real site; no separate walk-in class price is published, so none is invented. |
| Any claim that this demo replaces the whole company's web presence | Business is genuinely two-location (Newton Abbot + Torquay); both are covered honestly on classes.html rather than only building for one. |

## Asset Manifest
All five photos below are hosted on the business's own domain (karateacademy.co.uk/img/l/) at native 1793×1200px, JFIF-tagged "gd-jpeg", no visible third-party photographer watermark on any of them. Each is used in exactly one place (no reuse across sections).

| File | Source URL | Native size | License/credit | Watermark checked | Intended section | Copy match |
|---|---|---|---|---|---|---|
| assets/img/okinawa-training.jpg | http://karateacademy.co.uk/img/l/mi207.jpg (via karate_23.html / enjoy_karate_4.html gallery) | 1793×1200 | Business's own site photo | yes — none found | Home hero | Matches copy exactly: John Burke renshi (right, black belt) with his teacher Hokama Tetsuhiro Hanshi at a shrine in Okinawa, per the site's own caption text |
| assets/img/beach-black-belts.jpg | http://karateacademy.co.uk/img/l/mi815.jpg | 1793×1200 | Business's own site photo | yes — none found | Home gallery | Three black-belt students, used generically as "some of our senior black belts" (no names claimed since none are captioned on the source site) |
| assets/img/bunkai-practice.jpg | http://karateacademy.co.uk/img/l/mi736.jpg | 1793×1200 | Business's own site photo | yes — none found | Home "meet the chief instructor" section | Close bunkai/grip demonstration between two senior students, matches the bunkai/oyo emphasis described in copy |
| assets/img/seminar-chat.jpg | http://karateacademy.co.uk/img/l/mi84.jpg | 1793×1200 | Business's own site photo | yes — none found (background shows a partial venue poster, not a photo-credit watermark) | Home gallery | Captioned generically as an informal moment between sessions at a seminar, no names claimed |
| assets/img/kick-silhouette.jpg | http://karateacademy.co.uk/img/l/mi888.jpg | 1793×1200 | Business's own site photo | yes — none found | Home gallery | Generic silhouette practising a punch at sunset; captioned without naming any specific person since the source page doesn't identify the subject |

## Design Notes
- Palette: Deep indigo-navy ("tide") + vermillion/lacquer red ("shu") + warm sand paper, derived from the real hero photo's shrine curtain/lacquer-seal colours and the Devon beach photo — checked against sibling project iskf-hampstead-karate (ink-black + gold + sparse crimson) to avoid a re-skin; this build uses a cooler ink and a hotter red instead of gold. Full rationale in assets/css/tokens.css.
- Image layout pattern: `aspect-ratio` set directly on the `<img>` plus `height:auto` for the hero/gallery/bio photos (all at their real 1793:1200 ratio, no crop distortion); location cards on classes.html are deliberately text-only (no photo) since no verified own-asset photo of either venue building exists — cut rather than forced, per AGENTS.md step 5.
- Risk notes: Two verified own-domain emails exist (info@ and admin@karateacademy.co.uk); used info@ as primary throughout for consistency. Booking links (App Store, Google Play, goretrieve.com member portal) point to the business's real, currently-live third-party booking infrastructure — included as genuine utility, not fabricated.

## Builder QA
- Contrast: 0 violations across all 3 pages at mobile (390×844) and desktop (1440×1000) — see QA_REPORT.md
- Upscale mobile: 0 violations, 0 broken images (390×844)
- Upscale tablet: 0 violations, 0 broken images (834×1112)
- Upscale desktop: 0 violations, 0 broken images (1440×1000)
- Broken images: none found on any page/viewport
- Manual checks: text-overflow checked at 360px width against real longest strings (full address, full email, booking URLs) — no overflow on any checked box; see QA_REPORT.md for full detail
