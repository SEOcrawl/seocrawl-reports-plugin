# Report principles (shared by every SEOcrawl AI report)

Follow these rules in every report this plugin produces.

## Data honesty

- Use only numbers returned by SEOcrawl AI tools in this session. Never estimate, round up, or fill a gap with a guess.
- Pass raw numbers to the render tools (`render-scorecard`, `render-time-series-chart`, `render-data-table`). Let the widget format them.
- Read `data_freshness.last_complete_date` from each response and state it once near the top ("Data complete through 26 Sep 2026"). Search Console usually lags 2-3 days.
- When a tool returns an error such as `ga4_not_connected`, `feature_not_available`, `no_crawl_data` or `reporting_not_available`, skip that section and add one line in "Data notes" saying what is missing and how to enable it in SEOcrawl AI. Do not stop the whole report.
- Treat a zero in a previous period that predates `data_start_date` (GA4) as a coverage gap, not a real drop.
- Position: lower is better. A negative `position_delta` is an improvement. Say "improved from 23.5 to 17.3", never "position dropped".
- CTR from Search Console is a 0-1 fraction. Show it as a percentage with two decimals.

## Writing

- Lead with a 3-5 line executive summary a client can read in 20 seconds: the headline change, the main reason, and the single most important next step.
- Explain every chart or table in one or two plain sentences under it: what changed and why it matters.
- Always compare against the previous equal-length period unless the user asks for another comparison (month over month, year over year).
- End with 3-5 prioritized recommendations. Each one names the page, keyword, prompt or issue it applies to and the expected effect. No generic advice ("create quality content").
- Write in the language the user writes in, unless they ask for another language for the client.
- Call SEOcrawl AI's crawl feature "Crawler" in prose.

## Delivery

- Default: render the report inline in the conversation with the render tools, then write the narrative.
- When the user asks for a document, slide deck, PDF or a file to send to a client, build that format with whatever document tools are available, reusing the same numbers and narrative. Use the client's or agency's brand colors and logo when given; otherwise use a neutral palette of dark gray, light gray and one accent color.
- When the user wants a shareable SEOcrawl link and the property has a saved report, `list_reports` then `generate_report` returns a public snapshot URL. `generate_report` is a write action: tell the user before calling it.

## Credits

SEOcrawl AI tools consume account credits (each tool description lists its cost). Fetch only what the report needs, and avoid repeating the same call.
