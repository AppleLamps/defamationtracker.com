# /loomer/ — Investigation 002

Investigation 002 of the @ProjectConstitu defamation record. Subject: Laura Loomer
and Andrew Jacob Simpson. Window: 22 March 2026 to 5 May 2026.

The page at `loomer/index.html` is self-contained: inline CSS, inline data, single
HTML file. The data files in this folder are the source of truth for what gets
rendered on the page.

## Data files

### `loomer_campaign_full.json` — primary source of truth

A JSON array of 125 ranked @ProjectConstitu posts and replies that explicitly
reference Laura Loomer in the campaign window. Each entry is one post.

Schema:

| field | type | notes |
| --- | --- | --- |
| `post_id` | string | X status ID. Used to build `https://x.com/ProjectConstitu/status/{post_id}` |
| `url` | string | Canonical X URL |
| `created_at` | string | `M/D/YYYY H:MM` (UTC, no leading zeros) |
| `type` | string | `Post` or `Reply` |
| `view_count` | number | Platform-reported views |
| `favorite_count` | number | Likes |
| `repost_count` | number | Retweets |
| `reply_count` | number | Replies |
| `bookmark_count` | number | Bookmarks |
| `total_engagement` | number | Sum of likes, RT, replies, bookmarks |
| `categories` | string[] | All categorical tags (multi-label) |
| `per_se_categories` | string[] | Subset of `categories` that are defamation per se |
| `text` | string | Full original post text |

Category vocabulary (13 observed):

```
deadname_misgender         (32 posts) — "Larry" / misgendering
mental_health_attack       (17)       — psychiatric claims, suicide history
spousal_attack             (13)       — Andrew Jacob Simpson by name
sexual_misconduct          (9)        — per se
fraud_grift                (9)
appearance_attack          (7)
professional_disqual       (6)        — per se
citizenship_attack         (5)
religious_identity_attack  (5)
criminal_federal           (4)        — per se (DPPA, classified info)
drug_use                   (4)        — per se
stalking_accusation        (4)
third_party_business       (2)        — Tameron/Cannon Auto, State Farm
```

Per se categories (treated as inherently damaging under common-law defamation):
`criminal_federal`, `sexual_misconduct`, `professional_disqual`,
`mental_health_attack`, `drug_use`. The page treats `per_se_categories` as the
authoritative per se flag for each post.

### `LauraLoomer_tweets_2026_05_05.jsonl` — Loomer's account, used for §10

JSONL (one tweet per line). Schema:

```json
{"tweet_id":"...","text":"...","language":"en","type":"Tweet"|"Reply",
 "bookmark_count":0,"favorite_count":0,"retweet_count":0,"reply_count":0,
 "view_count":0,"created_at":"M/D/YYYY H:MM"}
```

The two posts cited in §10 (Pre-Litigation Notice) are:

- `2051808359397904453` — 5 May 2026 7:36 PM ET, reply to @ProjectConstitu
- `2051809111830872165` — 5 May 2026 7:39 PM ET, separate post

URLs are built as `https://x.com/LauraLoomer/status/{tweet_id}`.

### `ProjectConstitu_tweets-latest.jsonl` — full corpus, reference only

The full @ProjectConstitu corpus the campaign data was extracted from. Used to
verify counts and to find new exhibits when the campaign continues. Not loaded
by the page.

### `loomer_flagged_ranked.json` — earlier ranked subset, archival

Superseded by `loomer_campaign_full.json`. Kept for archival reproducibility.

## How the page consumes the data

The page does **not** fetch JSON at runtime. It inlines hand-picked exhibit
records into JS arrays in the page's `<script>` block. Each exhibit array is
named `EXHIBIT_*` and corresponds to one section of the page:

| Section | Array | Notes |
| --- | --- | --- |
| §03 Exhibit A | `EXHIBIT_A` | Currently pinned post (gold ribbon) |
| §04 Exhibit B | `EXHIBIT_B` | Deposition series, 4 cards |
| §05 Exhibit C | `EXHIBIT_C` | "Trump ENRAGED" |
| §07 Spousal | `EXHIBIT_SPOUSAL` | Andrew Jacob Simpson cards |
| §08 Mental Health | `EXHIBIT_MENTAL` | Suicide / commitment claims |
| §10 Pre-Lit Notice | `EXHIBIT_NOTICE` | Loomer's two C&D posts (red ribbon) |

Each entry copies the schema of `loomer_campaign_full.json` (or the Loomer JSONL
for §10), plus three optional UI flags:

- `pinned: true` — adds the gold "CURRENTLY PINNED" ribbon
- `notice: true` — adds the red "PRE-LITIGATION NOTICE" ribbon, switches the
  card header to red, links to `https://x.com/LauraLoomer/...` instead of
  ProjectConstitu, and renders `author: "@LauraLoomer"`
- `note: "..."` — editorial filed-exposure note rendered below the categories

The aggregate stats in the hero, §01, and §11 stat-blocks are hard-coded in HTML
and were derived from `loomer_campaign_full.json` directly. The two charts
(Figure 01 daily curve, Figure 02 category breakdown) likewise hard-code their
arrays in the `renderCharts()` function. The numbers in code comments next to
those arrays match what you get from running:

```bash
python3 -c "
import json, collections
d = json.load(open('loomer/loomer_campaign_full.json'))
print('total:', len(d))
print('per se:', sum(1 for x in d if x['per_se_categories']))
print('views:', sum(x['view_count'] for x in d))
print('eng:', sum(x['total_engagement'] for x in d))
c = collections.Counter()
for x in d:
    for cat in x['categories']:
        c[cat] += 1
print(c.most_common())
"
```

## Adding §13, §14, …

The page is structured for drop-in extension. To add a new section:

1. Append the relevant exhibit posts to a new `EXHIBIT_*` array in `<script>`.
2. Add a new `<section id="sNN">` block in HTML with the standard
   `.section-head` + `.exhibit-grid` + optional `.aside` pattern. Anchor IDs
   follow `s01` … `s99`.
3. Add `<a href="#sNN">Label</a>` to `.masthead-nav`.
4. If a new top-level chart is needed, add a `<canvas id="...">` and a Chart.js
   block in `renderCharts()` matching the existing colour and font defaults.
5. Call `renderInto("section-id", EXHIBIT_NEW)` at the bottom of `<script>`.

The exhibit auto-numbering counter (`exhibitCounter`) is global; new sections
will continue the numbering from where the previous section left off.

## Editorial constraints baked into this page

- The underlying lawsuit referenced in §04 is never named on this page. Any
  quote that mentions it is redacted with `[...]` markers when reproduced as
  evidence. New deposition-series entries should preserve this.
- No em dashes in editorial copy (post-text quotes are reproduced verbatim).
- Loomer is consistently described as a public figure, Andrew Jacob Simpson as
  a private citizen. The § 12 standards table depends on that distinction.

## Filing

Investigation 002 was filed 5 May 2026, the same day Loomer's pre-litigation
notice posts went live. New developments after that point should be added as
§13+ rather than appended to §10.
