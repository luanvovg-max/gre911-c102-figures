# C102 Static Category Pilot — OpenSpec/QA packet v0.1 (DRAFT)

**Prepared:** 2026-11-15 (Asia/Ho_Chi_Minh)
**Target:** https://c102.gre911.com/figure-1-64/ — **URL is a draft destination, not a verified live page**
**Source:** C102 Issue #3 SEO 90D; Foundation PR #4; Product Schema Audit PR #2.
**Stack:** branch `seo/c102-category-static-pilot-20261115` based on `seo/c102-90d-foundation-20261111` (PR #4). This PR MUST be reviewed after parent changes; never merge the child directly without checking branch topology.

## Objective
Demonstrate ONE independent, statically rendered category/guide URL on existing Vite + GitHub Pages with unique title, description, self-canonical and indexable HTML (rendered before client JS). Connect it from homepage via real `href` and list only genuinely implemented URLs in `sitemap.xml`. Preserve the immersive Runway and existing ImageKit setup. This pilot does NOT implement category catalogs, actual SKU PDPs, search demand volumes, paid media, pricing, offers or production deployment.

## Implementation
1. `public/figure-1-64/index.html` — stand-alone UTF-8 Vietnamese guide with one H1, helpful selection advice, breadcrumbs, mobile CSS, outbound link to existing homepage. Avoids unverified product, availability, reviews, brands and commerce claims; original provisional copy requires GRE Muse editorial QA.
2. `index.html` — adds one visible, crawlable link `href="/figure-1-64/"` to the new guide near the interactive Runway detail action. The original product filter mechanics and imported fixture remain unchanged.
3. `public/sitemap.xml` — includes root and pilot URL only, no unimplemented category or SKU URLs.
4. `.github/workflows/seo-pr-precheck.yml` — validates both generated HTML artifacts, canonicals, sitemap cardinality, homepage link and original CNAME; the workflow now applies to PRs targeting any branch to support stacked drafts.

## Source-of-truth & editorial gates
- Current `src/figures-data.json` is a source fixture, NOT Product Master; don't publish SKU-specific facts, prices, availability, compatibility or descriptions without canonical review.
- Sample numeric height 172.8 cm → 2.7 cm is an **arithmetic illustration**, NOT measurement of any C102 SKU.
- Generic usage content is a proposed editorial draft and must be reviewed by **GRE Muse (Meta-origin role, not Claude)**; no Muse ACK/approval or copyright clearance is implied by this code PR.
- `/figure-1-64/` is an informational/category guide. No Product/Offer/Review JSON-LD is added, consistent with Product Schema Audit PR #2.
- Brand claims remain provisional until GRE•911 brand/rights QA. Do not refer to miniature cars as GRE•911 merchandise.

## Test plan (NOT YET INDEPENDENTLY ACCEPTED)
- [ ] CI: `npm ci && npm run build` completed at **child PR HEAD**, with run URL and commit SHA.
- [ ] `dist/index.html`, `dist/figure-1-64/index.html`, `dist/sitemap.xml`, `dist/robots.txt`, `dist/CNAME` exist.
- [ ] HTML: unique title/H1 and self-canonical on category page; no `noindex`; Vietnamese lang; visible meaningful body text; real homepage-to-category and category-to-home links.
- [ ] Sitemap: exactly two URL entries, both return status 200 in preview and are canonical; no 404/redirect loops.
- [ ] Preview mobile 360px/768px and desktop 1440px: no overlap, layout or keyboard focus issue; Runway still behaves as before.
- [ ] Compare actual staging build with separate ImageKit fix; sample gallery images load correctly.
- [ ] HTTP live TLS, Cloudflare-managed robots response and GSC URL Inspection after any separately approved deploy; **UNKNOWN** until real receipts.
- [ ] Invalid SKU/category URL should return an actual 404, not soft-404, before any future PDP architecture is approved.
- [ ] GRE Muse content review; independent Claude/Codex technical QA; GREDO/BosX receipts only via their real channels.
- [ ] Exact reviewed child SHA and Founder GO, with rollback SHA, before release/merge; no inherited GO from parent PR.

## Known blockers
- Production website access via general-purpose web retrieval was unavailable on 2026-11-15; this is **tool access failure**, NOT evidence of site outage.
- No authenticated GSC/GA4 baseline; non-branded organic clicks, impressions, CTR and index count UNKNOWN.
- GRE Muse reported delivery of `c102-seo-30d-board` v1 in Issue #3 comment, but no accessible artifact link was found in the C102 repository; independent Muse task-transport/ACK remains UNKNOWN.
- Foundation PR #4 itself is still draft, and there are no independent review approvals on PR #2 or #4 as of this check.

## Release decision
**DRAFT_NOT_DEPLOYED / NO_GO production.**
This is one code pilot, not completion of the 4–6 category pages or 8+ product pages in the 90D scorecard. Keep Muse/BosX/GREDO ownership and no competing AI dispatcher.
