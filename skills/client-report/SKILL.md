---
name: client-report
description: >
  This skill should be used when an agency or consultant asks for a "client report",
  "monthly client report", "white-label SEO report", "SEO and GEO report for my client",
  "full report for client X", "reports for all my clients" or "executive SEO summary for the board".
  Combines Search Console performance, AI visibility and Crawler health from SEOcrawl AI into one
  branded, client-ready report, and can run it for several clients in a row.
metadata:
  version: "0.1.0"
---

# Agency client report (SEO + GEO + technical)

Combine the three SEOcrawl AI report types into one client-ready report written for a non-technical reader, and repeat it across several clients when asked.

Read `references/report-principles.md` before writing.

## 1. Set up the report

Collect, asking only for what is missing:

- **Client site(s)**: resolve each with `list_properties`. "All my clients" means every property the user can access; list them and confirm before running more than five, because each report spends credits.
- **Period**: default last 28 days compared with the previous 28 days; accept a calendar month.
- **Audience**: marketing manager (default), executive or technical team. Executives get a one-page summary; technical teams get the full issue tables.
- **Branding**: agency name, logo and colors for a white-label report. Never add SEOcrawl AI branding to a white-label report unless the user asks.
- **Format**: inline in the conversation (default), or a document, slide deck or PDF when the user asks.

## 2. Gather the data

Follow the data steps of the three focused skills in this plugin, using a lighter call set per section:

- **Organic search** (from `seo-performance-report`): `get_gsc_summary`, `get_top_pages` (limit 5), `list_winners_losers` for keywords (direction `both`, limit 5), quick-win keywords (positions 4-15, limit 5).
- **AI visibility** (from `ai-visibility-report`): `get_ai_visibility_share_of_voice`, `get_ai_visibility_mentions`, and `get_ga4_ai_referrers` when GA4 is connected. Skip the section with a one-line note when the property has no AI Tracker prompts.
- **Technical health** (from `crawler-health-report`): pick the main Crawler project with `list_site_audit_projects`, then `get_site_audit_summary` and `compare_site_audit_crawls` with that `auditId`, plus the top 5 issues from `list_site_audit_issues`.

## 3. Write the report

1. **Cover**: client name, period, agency name.
2. **One-page summary**: three traffic-light lines (Organic search, AI visibility, Technical health), each with the headline number, its change, and one sentence of meaning. Then "What we did" (ask the user for completed work, or pull it from `list_annotations` and `list_tasks` for the period when available) and "What we do next".
3. **Organic search**: scorecard, top pages, winners and losers, quick wins.
4. **AI visibility**: share of voice ranking, mention breakdown, AI referral traffic, the prompts to win next.
5. **Technical health**: health score and change, what changed since the last crawl, top fixes.
6. **Next month's plan**: 3-5 prioritized actions across the three areas.
7. **Appendix / data notes**: sources, freshness dates, missing data.

Keep the language free of jargon: say "people clicking through from Google" rather than "organic CTR" in the summary, and explain position as "average ranking on Google (lower is better)".

## 4. Several clients

When running for several clients, produce one complete report per client, then finish with a portfolio table: client, clicks change, AI share of voice change, health score change, and the one action each. Flag any client with a drop larger than 20% in clicks or 10 points in health score at the top.
