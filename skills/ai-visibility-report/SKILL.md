---
name: ai-visibility-report
description: >
  This skill should be used when the user asks for a "GEO report", "AI visibility report",
  "LLM visibility report", "share of voice in ChatGPT", "how visible is my brand in AI answers",
  "AI Tracker report", "brand mentions in ChatGPT, Claude, Gemini or Perplexity",
  "which competitors win in AI search" or "AI referral traffic report". Builds a
  Generative Engine Optimization report from live SEOcrawl AI Tracker and GA4 data.
metadata:
  version: "0.1.0"
---

# GEO / AI visibility report

Build a report on how a brand appears in AI assistant answers (ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews) and how much traffic those assistants send, from live SEOcrawl AI data.

Read `references/report-principles.md` before writing.

## 1. Resolve the property and period

Resolve the property with `list_properties` and the period as in any SEOcrawl AI report. Default to `last_28_days`. AI Tracker data is weekly or daily depending on the plan, so prefer 28 days or longer for trends.

## 2. Fetch the data

| Section | Tool | Notes |
|---|---|---|
| Share of voice and competitor ranking | `get_ai_visibility_share_of_voice` | Returns `own_domain`, `ranking` (share of voice 0-100 plus change) and `competitor_comparison` (prompts won and lost against the top competitor) |
| Mention breakdown | `get_ai_visibility_mentions` | Mentioned prominently / only in text / absent, as counts and percentages. These are presence buckets, not sentiment |
| Visibility trend | `get_ai_visibility_history` | Daily series. If `insufficient_history` is true, skip the chart and say so |
| Tracked prompts | `list_prompts` | limit 50 |
| Per-prompt detail (top 3-5 prompts that matter most) | `get_prompt_detail` | Per-engine mention rate |
| Citations for the lost prompts | `list_prompt_citations` | Which domains and URLs the engines cite instead of the brand |
| AI referral traffic | `list_ga4_properties`, then `get_ga4_ai_referrers` | Sessions and key events by engine. Skip on `ga4_not_connected` |

If `list_prompts` returns no prompts, stop and tell the user the property has no AI Tracker prompts yet, then offer to create a starter set with `create_prompt` (a write action: confirm the prompt list with the user first).

## 3. Assemble the report

1. **Title and scope**: brand, domain, period, engines covered.
2. **Executive summary**: the brand's share of voice and rank among tracked domains, the change versus the previous period, the strongest and weakest engine, and the one prompt cluster to win next.
3. **Scorecard**: `render-scorecard` with share of voice (%), rank position, prompts mentioned (%), prompts absent (%), AI referral sessions when available.
4. **Share of voice ranking**: `render-data-table` with domain, share of voice, change. Mark the brand's own row in the narrative.
5. **Visibility trend**: `render-time-series-chart` with the history series.
6. **Head to head**: prompts won and lost against the top competitor, as two short lists.
7. **Who gets cited instead**: for the lost prompts, the domains and URLs the engines cite. Group them by type (competitor product pages, review sites, video, forums, media) because each type needs a different tactic.
8. **AI referral traffic**: table by engine with sessions, key events and change.
9. **Recommendations**: 3-5 GEO actions tied to specific prompts and cited sources, for example:
   - Get listed or reviewed on the third-party pages the engines cite for a lost prompt.
   - Publish or restructure a page that answers the exact prompt with clear, quotable statements, comparison tables and FAQ blocks.
   - Add structured data and keep facts (pricing, features, locations) consistent across the site and profiles.
   - Track the missing prompts or engines when coverage is thin.
10. **Data notes**: engines without data, prompts with too few runs, GA4 not connected.

## Interpreting the numbers

- Share of voice is an average visibility across tracked prompts, so a small prompt set moves a lot. State the number of prompts behind it.
- A brand can be mentioned without being cited (linked). Report both when the data allows, and treat citation of the brand's own URLs as the stronger signal.
- Engines sometimes cite pages unrelated to the topic (for example, a gaming list for a question about "MCP servers"). Leave off-topic citations out of the "who gets cited instead" analysis.
- Leave prompts with only one run in the period, or prompts whose text marks them as a test, out of per-prompt conclusions, and mention them in Data notes.
- The last point of the visibility trend can cover an incomplete run. Do not call a drop on the final point a trend unless the previous points confirm it.
- Video platforms and review sites ranking above the brand usually mean the engines rely on third-party sources for that topic; owned content alone will not close the gap.
