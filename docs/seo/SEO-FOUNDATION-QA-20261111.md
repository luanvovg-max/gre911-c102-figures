# C102 SEO Foundation — Change & QA packet (DRAFT)
Date: 2026-11-11 (Asia/Ho_Chi_Minh)
Tracking: https://github.com/luanvovg-max/gre911-c102-figures/issues/3

## Objective
Add discoverability signals for the EXISTING homepage only. This is a safe first patch, **not** a complete solution for organic acquisition. The largest gap is still distinct crawlable category/PDP URLs, which need a verified Product Master mapping and static-render architecture before publishing.

## Proposed source changes
- `index.html`: on-page title/description rewritten around figure 1:64 / sa bàn / gara / phố Nhật; add single homepage self-canonical, social OG tags; no unverified product/price/stock claims.
- `public/robots.txt`: allow crawling; discover sitemap.
- `public/sitemap.xml`: ONE confirmed canonical homepage URL; no fictitious category/product URLs.
- Preserve GitHub Pages `public/CNAME` and all C102 interactive Runway logic unmodified.

## Evidence / observed baseline
- `main` initial `index.html` had one H1, title + description but no canonical or JSON-LD; static menu uses `href="#"` for filter actions; no separate visible product URLs.
- `public/robots.txt` and `public/sitemap.xml` were 404 in GitHub contents API prior to patch.
- Direct site fetch and HTTP responses were NOT verified; browser-side loading/crawl remains **UNKNOWN**.
- GSC Wizard `subscription_required`, Semrush `no_api_units`; traffic baseline and search volume **UNKNOWN**.
- `src/figures-data.json` is a repo fixture, NOT authenticated Product Master.
- Separate ImageKit fix branch exists; do not merge collisions blindly.

## Developer pre-check — NEEDS EXECUTION
1. `npm ci && npm run build`.
2. Check `dist/index.html` has exactly ONE canonical `https://c102.gre911.com/`; description/title correct, UTF-8 valid.
3. Check `dist/robots.txt` and `dist/sitemap.xml` exist in built output, XML valid, URLs absolute and point to existing canonical homepage.
4. Confirm `public/CNAME` copies into `dist/CNAME`; production custom domain unchanged.
5. Compare `index.html` with pending ImageKit branch; resolve overlaps before any integration.
6. Browser + mobile QA on staged preview; no Runway regressions.
7. Independent SEO inspection: Google URL Inspection, Rich Results Test if markup later, production HTTP/TLS canonical/robots/sitemap, Search Console sitemap submission only with appropriate permission.
8. Verify no accidental sensitive tracking data, fake Product/Offer/Review schema, or noncanonical URLs.

## Independent review and GO
Author self-check is NOT acceptance. Assigned independent reviewer must attach build log, screenshot, tests, before/after and rollback SHA. Founder GO required for exact reviewed SHA before pushing changes onto main. GitHub Pages deployment triggers from main.
Status: DRAFT_NOT_DEPLOYED.
