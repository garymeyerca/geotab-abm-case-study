# Geotab Case Study — Site

Two-page static site demonstrating how a job application becomes an ABM campaign. The Geotab Senior Manager, Demand Generation role is the worked example.

| Page | URL | What it is |
|------|-----|-----------|
| `index.html` | `/` | **The case study** — the strategy end-to-end, the marketing psychology in play (11 principles annotated), every move explained. Bold editorial design (dark charcoal + cream + rust + gold). The primary deliverable. |
| `audit.html` | `/audit` | **The worked artifact** — the actual 2,500-word demand-gen audit that the case study references. NYT-op-ed serif aesthetic, restrained. Acts as the proof-of-craft inside the case study. |

Both pages are public, indexable, and shareable. The campaign described was designed but never executed — the emails were drafted, the targets identified, the sequence planned, but nothing was sent. The artifact is the demonstration.

## Deploy

```bash
cd /home/dev/geotab/site
npx vercel deploy --yes
```

First run, it'll prompt to log in (browser flow). After that, returns a `*.vercel.app` URL. The site deploys both pages in a single command. `cleanUrls: true` in `vercel.json` handles `/audit` → `audit.html` and `/` → `index.html` routing.

Attach a custom domain later:
```bash
npx vercel domains add yourdomain.com
npx vercel alias <deployment-url> yourdomain.com
```

## Optional: wire up analytics

`audit.html` currently has a Plausible script with placeholder `data-domain="REPLACE_WITH_VERCEL_DOMAIN"`. After deploy:

1. Sign up at [plausible.io](https://plausible.io) — add your `.vercel.app` (or custom) domain
2. Edit `audit.html`, replace `REPLACE_WITH_VERCEL_DOMAIN` with the actual domain
3. Add the same script to `index.html` if you want the case-study reads tracked too
4. Redeploy: `npx vercel --prod`

Plausible is privacy-friendly (no cookies, no GDPR banner) and gives you page views, referrer breakdown, and rough geographic data. If you'd rather skip analytics, delete the `<script>` line.

## Local preview before deploy

```bash
cd /home/dev/geotab/site
python3 -m http.server 8000
# open http://localhost:8000        → the case study
# open http://localhost:8000/audit  → the worked artifact
```

Or open `index.html` and `audit.html` directly in a browser.

## Design notes

**Index (the case study):** dark editorial. Cream-on-charcoal main with inverse cream sections breaking the rhythm. Rust + gold accents. Big typographic hero with strike-through pattern interrupt. Nine chapters, each with its own visual section: timeline diagrams, target profile cards, principle callouts with academic sources, stat blocks, artifact grid.

**Audit (the worked artifact):** restrained NYT-op-ed serif. Warm off-white (#faf8f3) with deep ink and single accent (#8a2a0c). 680px max-width single column. Generous vertical rhythm. Print stylesheet included — saves as a clean PDF.

Both pages: serif body (Iowan Old Style → Charter → Georgia stack, no webfont loading required); Inter for meta/labels; system stack monospace for code.

## Source files referenced by the case study

The case study's "Artifacts" chapter links to internal workspace files. These aren't deployed but exist at `/home/dev/geotab/`:

- `audit/audit.md` — the audit's markdown source (rendered as `audit.html`)
- `targets/leadership.md` — ranked target list (Roula, Charlie, Carrie, + B-list)
- `research/footprint-audit.md` — Geotab marketing footprint evidence
- `outreach/01-roula-vrsic.md` — Roula sequence (email + LinkedIn + bump, drafted)
- `outreach/02-charlie-elliott.md` — Charlie sequence
- `outreach/03-carrie-lepage.md` — Carrie sequence
- `outreach/sequence-overview.md` — send order, pre-flight, contingency tree

If you want any of those references to resolve to live URLs on the site, copy them into `site/` and add HTML wrappers.

## What to test after deploy

1. Open `/` on a phone. The big editorial hero needs to scale cleanly — typography clamps should keep it readable at 360px wide.
2. Open `/audit` on a phone. Should hit ~17px body, single column, no horizontal scroll.
3. Click the "Read the full audit" link in Move 02 of the case study — confirm it lands at `/audit` cleanly.
4. Test the artifact-grid links in Chapter 08 — confirm `/` and `/audit` resolve.
5. Tap any `mailto:` link in the audit — confirm it opens a mail client.
6. Share `/` on LinkedIn (drafts, not posted) — confirm the OG preview reads cleanly.
