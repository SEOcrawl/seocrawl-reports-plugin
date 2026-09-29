---
name: seo-performance-report
description: >
  This skill should be used when the user asks for an "SEO report", "monthly SEO report",
  "Search Console report", "organic performance report", "how did my SEO do this month",
  "clicks and impressions report", "top keywords and pages report" or "winners and losers"
  for a website connected to SEOcrawl AI. Builds a client-ready organic search performance
  report from live Google Search Console data.
metadata:
  version: "0.1.0"
---

# SEO performance report

Build a client-ready organic search report for one property from live SEOcrawl AI (Google Search Console) data.

Read `references/report-principles.md` before writing. It sets the rules for data honesty, writing and delivery that apply to every section below.

## 1. Resolve the property and period

1. Identify the site. If the user named a domain, match it against `list_properties` (accept `example.com`, `www.example.com` or `sc-domain:example.com`). If several properties exist and none was named, ask which one with a short list.
2. Identify the period. Map "this month / last month / last 28 days" to a preset (`last_28_days`, `last_3_months`, `last_year`) or to absolute `from`/`to` dates. For a calendar month, use absolute dates and compare with the previous calendar month through `compare_date_ranges`. Default to `last_28_days`.

## 2. Fetch the data

Run these calls (independent calls can run in parallel):

| Section | Tool | Key arguments |
|---|---|---|
| KPIs | `get_gsc_summary` | property, period |
| Month-over-month or custom comparison (only when asked) | `compare_date_ranges` | periodA / periodB |
| Top pages | `get_top_pages` | sortBy `clicks`, limit 10 |
| Top keywords | `get_top_keywords` | sortBy `clicks`, limit 15 |
| Keyword winners and losers | `list_winners_losers` | dimension `keywords`, metric `clicks`, direction `both`, limit 10 |
| Page winners and losers | `list_winners_losers` | dimension `pages`, metric `clicks`, direction `both`, limit 10 |
| Quick wins | `get_top_keywords` | positionMin 4, positionMax 15, sortBy `impressions`, limit 10 |

Optional sections, only when relevant or asked:

- Indexation: `get_indexation_summary` (skip on `feature_not_available`). Follow the tool description's caveats on `known_urls` versus `sitemap_urls`.
- Google Discover: `get_discover_summary` when the site is a publisher or Discover clicks are non-zero.
- GA4 engagement: `list_ga4_properties`, then `get_ga4_summary` when the property has GA4 connected.
- Country or device slice: pass `country` (ISO alpha-3) or `device` to the calls above.

## 3. Assemble the report

Use this structure:

1. **Title and scope**: site, period, comparison period, data freshness date.
2. **Executive summary**: 3-5 lines.
3. **KPI scorecard**: `render-scorecard` with clicks, impressions, CTR (%), average position, each with change versus the previous period. When `branded_breakdown` is present, add branded and non-branded clicks as a second group.
4. **Top pages**: `render-data-table` (URL, clicks, change %, impressions, position) plus two sentences.
5. **Top keywords**: `render-data-table` (keyword, clicks, change %, position, search volume when available).
6. **Winners and losers**: one table each for keywords and pages, with the likely reason for the biggest moves (seasonality, a ranking change, a page that lost impressions).
7. **Quick wins**: keywords ranking 4-15 with high impressions, and the page to improve for each.
8. **Optional sections** from step 2.
9. **Recommendations**: 3-5 prioritized actions.
10. **Data notes**: missing sources, freshness, filters applied.

Pass `source={tool, params}` to each render call so the widget links back to SEOcrawl AI.

## Interpreting common patterns

- Impressions down and position improved: usually fewer low-quality long-tail impressions. Check whether clicks held; if so, call it a healthy consolidation.
- Clicks flat and impressions up: a CTR problem. Point to the pages with the most impressions and the lowest CTR and suggest title and meta description tests.
- A single page driving most of a drop: open `get_page_detail` for that URL before explaining it.
