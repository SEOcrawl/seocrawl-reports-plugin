# SEOcrawl AI Reports

Client-ready SEO and GEO reports in Claude, built from your live SEOcrawl AI data.

Ask in plain language, for example "make the September SEO report for example.com" or "how visible is my brand in ChatGPT compared with my competitors?", and Claude pulls the numbers from your SEOcrawl AI account, renders charts and tables, explains what changed and why, and ends with prioritized next steps. It can also turn the result into a document, slide deck or PDF for your client.

## What's included

| Skill | What it builds |
|---|---|
| **SEO performance report** | Google Search Console clicks, impressions, CTR and position versus the previous period, top pages and keywords, winners and losers, quick-win keywords, and optional indexation, Google Discover and GA4 sections. |
| **GEO / AI visibility report** | Your brand's share of voice in AI assistant answers (ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews) against competitors, mention breakdown, visibility trend, prompts won and lost, the sources the engines cite instead of you, and AI referral traffic from GA4. |
| **Crawler health report** | Technical health score, issues by severity and category, what changed since the previous crawl, and an action plan with example URLs. |
| **Agency client report** | All three combined into one branded, jargon-free report for a client, with a one-page summary and next month's plan. Can run across several clients and finish with a portfolio overview. |

## Requirements

- A SEOcrawl AI account with at least one Google Search Console property connected. Sign up at [seocrawl.ai](https://seocrawl.ai).
- AI visibility sections need AI Tracker prompts on the property. AI referral traffic needs Google Analytics 4 connected. Technical sections need at least one completed Crawler run. Missing sources are skipped with a note; the rest of the report still runs.
- The first time a skill runs, Claude asks you to sign in to SEOcrawl AI (OAuth). No API keys are stored in this plugin.

## How it works and what data it uses

This plugin contains instructions (skills) and one connector declaration. It does not run local code, scripts or hooks.

- **Connector**: the SEOcrawl AI MCP server at `https://mcp.seocrawl.ai` (the same server listed in the Claude connector directory). All report data is read from your SEOcrawl AI account through this connector, after you sign in with OAuth.
- **Data sent**: tool calls send the property, date range and filters you ask for to SEOcrawl AI. The plugin sends nothing to any other service.
- **Write actions**: the skills only read data, with three exceptions that Claude confirms with you first: `generate_report` (creates a public snapshot link of a saved SEOcrawl report), `create_prompt` (adds AI Tracker prompts) and `trigger_site_audit_crawl` (starts a crawl).
- **Credits**: SEOcrawl AI tools use your account credits; each tool lists its cost. The skills fetch only what each report needs.

Privacy policy: [seocrawl.ai/legal/privacy-policy](https://seocrawl.ai/legal/privacy-policy). Terms: [seocrawl.ai/legal](https://seocrawl.ai/legal). Connector documentation: [seocrawl.ai/mcp](https://seocrawl.ai/mcp).

## Example prompts

- "Build the monthly SEO report for example.com for September, compared with August."
- "GEO report for example.com: where do we stand against competitors in ChatGPT and Perplexity?"
- "What broke on example.com since the last crawl? Give me a fix list for the developers."
- "Make a white-label client report for acme.com with our agency logo, as a slide deck."
- "Run the client report for all my clients and give me a portfolio summary."

## Support

Questions or issues: info@seocrawl.com. Published by SEOcrawl SL, Andorra.

## License

MIT. See [LICENSE](LICENSE).
