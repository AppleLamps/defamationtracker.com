# defamationtracker.com

Defamation Tracker is a static documentation site: data-driven case studies of coordinated smear-style campaigns on social platforms. The first published investigation focuses on @ProjectConstitu; the site is structured so additional accounts can be added over time.

Live site: [defamationtracker.com](https://defamationtracker.com/). Deployed on [Vercel](https://vercel.com) as static files (no build step).

## Repository layout

| File | Purpose |
|------|---------|
| `index.html` | One-page report: hero, charts (Chart.js), tweet grids, timeline, legal section |
| `page-local.css` | Supplemental styles (pulled from former inline styles) |
| `og.jpg` | Open Graph / Twitter card image (1200×630); must live at site root next to `index.html` |
| `favicon.svg` | Tab icon |
| `robots.txt` | Allows crawlers; points to sitemap |
| `sitemap.xml` | Single URL today (`/`); bump `<lastmod>` when you materially change the page |
| `.gitignore` | Common ignores (`node_modules`, `.env`, `.vercel`, editor/OS noise) |

## Stack

- Plain HTML and CSS; Chart.js from cdnjs for charts.
- SEO: meta description, canonical URL, Open Graph, Twitter Card (`summary_large_image`), JSON-LD (`WebSite`, `Organization`, `WebPage`, `Article`). After large edits, try [Rich Results Test](https://search.google.com/test/rich-results) and rescrape in [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/) if previews are stale.

## Local preview

From the project root:

```bash
npx serve .
```

Open the URL the CLI prints; the homepage is `/` (or `/index.html`).

## Deploy on Vercel

1. Push this repo to GitHub, GitLab, or Bitbucket.
2. New Project → import the repo. Root directory: `.`
3. Framework preset: Other (static). No install or build commands unless you add a toolchain later.
4. Domains: add `defamationtracker.com` under Project → Settings → Domains and finish DNS in Vercel’s flow.

CLI: `npm i -g vercel`, then `vercel` or `vercel --prod`.

Google Search Console: add the property and submit `https://defamationtracker.com/sitemap.xml` after launch or major updates.

## License

Add a `LICENSE` file if you publish the repo under explicit terms.
