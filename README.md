# defamationtracker.com

Defamation Tracker is a static documentation site: data-driven case studies of coordinated smear-style campaigns on social platforms. The first published investigation covers [@ProjectConstitu](https://x.com/ProjectConstitu); the site is built to expand to additional accounts over time.

Live site: [defamationtracker.com](https://defamationtracker.com/). Deployed on [Vercel](https://vercel.com) as static files (no build step).

## Repository layout

```text
.
├── index.html              # Active homepage served at /
├── favicon.svg             # Browser tab icon; keep at site root
├── og.jpg                  # Open Graph / Twitter card image; keep at site root
├── robots.txt              # Crawler rules; references sitemap.xml
├── sitemap.xml             # Single URL sitemap for /
├── data/
│   ├── lunarcrush/         # LunarCrush reach, peak-day, and top-post exports
│   └── x-export/           # X API exports and tracker review CSVs
├── archive/
│   ├── index-backup.html   # Previous homepage design before V2 was promoted
│   └── page-local.css      # CSS used by the archived backup page
└── README.md
```

`index-v2.html` was removed. It was only a temporary copy while V2 was being promoted, and keeping it created drift risk.

## Stack

- Plain HTML + CSS. Chart.js is loaded from cdnjs.
- V2 uses a sticky editorial masthead, equal-column desktop nav, horizontal-scroll mobile nav, filing metadata strip, print-style stat rows, charts, exhibit cards, category filters, chronology, and legal context.
- Responsive behavior: safe-area padding via `env(safe-area-inset-*)`, mobile-tightened typography and spacing, adaptive chart heights, and touch-friendly nav/tab targets.
- SEO: meta description, canonical URL, Open Graph with `og.jpg`, and Twitter Card (`summary_large_image`).
- Safari compat: `-webkit-backdrop-filter` on the sticky masthead.

## Page editing

`index.html` is the only live page. Make page edits there.

The archived page in `archive/` is kept only for reference. Do not edit it unless you are intentionally restoring or comparing against the old design.

Evidence exports live under `data/` so the root stays deployment-focused.

## Local preview

```bash
npx serve .
```

Open the URL printed; the homepage is `/`.

## Deploy on Vercel

1. Push to GitHub / GitLab / Bitbucket.
2. New Project → import repo. Root directory: `.`
3. Framework preset: Other (static). No build commands needed.
4. Add `defamationtracker.com` under Project → Settings → Domains.

CLI: `npm i -g vercel`, then `vercel` or `vercel --prod`.

## Post-launch

- **Google Search Console:** add the property and submit `https://defamationtracker.com/sitemap.xml`.
- **Social previews:** rescrape via [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/) after updating `og.jpg` or meta tags.
- **Structured data:** validate with [Rich Results Test](https://search.google.com/test/rich-results).

## License

Add a `LICENSE` file if you publish the repo under explicit terms.
