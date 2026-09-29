---
name: crawler-health-report
description: >
  This skill should be used when the user asks for a "technical SEO report", "site health report",
  "Crawler report", "site audit report", "what broke since the last crawl", "technical issues report",
  "health score report" or "crawl comparison" for a site connected to SEOcrawl AI. Builds a
  prioritized technical health report from the latest SEOcrawl AI Crawler runs.
metadata:
  version: "0.1.0"
---

# Crawler health report

Build a prioritized technical SEO report from SEOcrawl AI's Crawler data, focused on what to fix first and what changed since the previous crawl.

Read `references/report-principles.md` before writing.

## 1. Pick the right crawl

A property can have several Crawler projects, including small test or list crawls. Do not rely on the default.

1. Call `list_site_audit_projects`.
2. Choose the project that represents the whole site. If the user named a project, use that one. Otherwise:
   - Keep only projects with status `completed` (a `running` or `stuck` project has a partial crawl).
   - Prefer mode `complete` or `organic_traffic` over `list`, and skip projects whose name marks them as a test or QA run.
   - Among those, take the one with the most `pages_crawled`; break ties by the newest `last_crawl_date`.
   - Name the chosen project in the report so the reader knows which crawl it covers.
3. Pass its `audit_id` as `auditId` to every call below.
4. If no project has completed a crawl, say so and offer to start one with `trigger_site_audit_crawl` (a write action that spends credits: confirm first).

## 2. Fetch the data

| Section | Tool | Notes |
|---|---|---|
| Health score, severities, categories | `get_site_audit_summary` | `health_score.current/prev/delta`, `issues_by_severity`, `issues_by_category`, `category_scores` |
| Issue list | `list_site_audit_issues` | limit 50, most severe first. Includes `how_to_fix` and `priority` |
| Example URLs for the top issues | `list_site_audit_issue_pages` | Only for the 3-5 issues in the action plan, limit 5 each |
| Changes since the previous crawl | `compare_site_audit_crawls` | Skip on `no_previous_crawl` |
| Business impact (optional) | `get_top_pages` | Cross-check whether affected URLs are among the pages that earn clicks |

In the report, "critical" is the Crawler's error level.

## 3. Assemble the report

1. **Title and scope**: site, Crawler project name, crawl date, pages crawled, previous crawl date.
2. **Executive summary**: health score and change, number of critical issues, the most important regression, and the first fix.
3. **Scorecard**: `render-scorecard` with health score, pages crawled, critical, warning and notice counts (with change when a previous crawl exists), then a second group with the category scores.
4. **What changed**: new, resolved, worsened and improved issues from `compare_site_audit_crawls`.
5. **Action plan**: a `render-data-table` of the top issues ranked by severity, pages affected and business impact, with the fix guidance in plain language and 2-3 example URLs each.
6. **By category**: one short paragraph per category with issues (On-page, Links, International, Structured data, Sitemaps, Technical, Social).
7. **Recommendations**: the 3-5 fixes to ship first, with owner type (developer, content, SEO) and expected effect.
8. **Data notes**.

## Prioritizing

Rank an issue higher when it is critical, affects many pages, affects pages that earn organic clicks, or blocks indexing (noindex, broken canonicals, 4xx/5xx, robots blocks, broken hreflang on international sites). Rank cosmetic social-tag issues last unless the user cares about social sharing.
