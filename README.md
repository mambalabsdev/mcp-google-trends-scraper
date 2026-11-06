# Google Trends Scraper MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-google-trends-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-trends-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-google-trends-scraper)](https://github.com/mambalabsdev/mcp-google-trends-scraper/blob/main/LICENSE)

An MCP server for the Mamba Labs Google Trends Scraper actor on Apify. Read Google Trends for your search terms and get a summary row per term plus flat detail rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give search terms, with a geo, timeframe, category, or search property if you want them. For each term you get one summary row (average, latest, and peak interest, recent change, top region, top and rising queries) and flat rows for interest over time, regions, and related queries. Terms can be compared in groups of 5 on one scale. Values are relative to the chart, not search counts.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-google-trends-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-google-trends-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_google_trends` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "How has interest in matcha changed in the US over the last 12 months?"
- "Compare coffee and tea by US metro area."
- "What are the rising search queries around ozempic?"

## Tool

`get_google_trends`. One tool. Example arguments:

```json
{
  "terms": [
    "coffee",
    "matcha"
  ],
  "geo": "US",
  "timeframe": "today 12-m"
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `terms` | array | no | Search terms, up to 200 per run. A Google topic ID such as /m/0j6n6s8 also works. |
| `geo` | string | no | Empty for worldwide, or a Google geo code such as US, DE, or US-CA. |
| `timeframe` | string | no | now 1-H, now 4-H, now 1-d, now 7-d, today 1-m, today 3-m, today 12-m, today 5-y, all, or two dates as YYYY-MM-DD YYYY-MM-DD. |
| `category` | string | no | Google Trends category number. 0 is all categories. The number is in the cat= part of a Trends URL. |
| `property` | string | no | Which Google search to read: web, news, images, youtube, or shopping. |
| `compare` | boolean | no | true: terms are charted together in groups of 5, on one shared scale. false: each term gets its own chart. Each term is charged either way. |
| `outputs` | array | no | Any of interest_over_time, interest_by_region, related_queries, related_topics. Default is the first three. See Limitations for related_topics. |
| `region_resolution` | string | no | auto: countries worldwide, regions inside a country. DMA is US metro areas. CITY needs a geo. COUNTRY needs an empty geo. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |
| `proxy_mode` | string | no | auto (default): requests go through the Apify proxy pool, and a request the pool cannot get through is retried on residential addresses. residential: every request uses residential addresses. A reliability setting only; it does not change what you pay. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `term-report`, one search term with data, summary row plus up to 170 detail rows | $0.0065 | $0.006175 | $0.00585 | $0.005525 |
| `extra-rows`, each further 100 detail rows on one term past the first 170 | $0.0035 | $0.003325 | $0.00315 | $0.002975 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

A term with no data, a term Google refused, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/google-trends-scraper](https://apify.com/mambalabs/google-trends-scraper)

## License

MIT. See [LICENSE](LICENSE).
