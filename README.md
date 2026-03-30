# defamationtracker.com

Defamation Tracker is a static documentation site: data-driven case studies of coordinated smear-style campaigns on social platforms. The first published investigation covers [@ProjectConstitu](https://x.com/ProjectConstitu); the site is built to expand to additional accounts over time.

Live site: [defamationtracker.com](https://defamationtracker.com/). Deployed on [Vercel](https://vercel.com) as static files (no build step).

## Repository layout

| File | Purpose |
| --- | --- |
| `index.html` | One-page report: hero, interactive charts (Chart.js), tweet grids with category filters, escalation timeline, legal analysis |
| `page-local.css` | Supplemental styles extracted from former inline styles |
| `og.jpg` | Open Graph / Twitter card image (1200×630); must stay at site root |
| `favicon.svg` | SVG tab icon |
| `robots.txt` | Allows crawlers; references `sitemap.xml` |
| `sitemap.xml` | Single URL (`/`); bump `<lastmod>` after material changes |
| `.gitignore` | Ignores `node_modules`, `.env`, `.vercel`, editor/OS files |

## Stack

- Plain HTML + CSS. Chart.js loaded from cdnjs.
- Mobile-first responsive: `100svh` hero for iOS Safari, `env(safe-area-inset-*)` for notch/Dynamic Island, horizontal-scroll nav, 44px touch targets, adaptive chart layouts (shorter labels, bottom legend on small screens).
- SEO: meta description, canonical URL, Open Graph with `og.jpg`, Twitter Card (`summary_large_image`), JSON-LD graph (`WebSite`, `Organization`, `WebPage`, `Article`).
- Safari compat: `-webkit-backdrop-filter` on sticky nav.

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
