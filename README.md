# Data Gleaner MCP server

Five web data tools for AI agents, served as one remote MCP server: website contact details, Google Hotels prices, Weibo posts, Bilibili videos and comments, and sitemap URLs. You pay per result returned, nothing to install and nothing to host.

The tools are [Data Gleaner](https://apify.com/datagleaner) Actors on Apify, served through Apify's official hosted MCP server at `mcp.apify.com`. This repository holds the connection configs and the registry metadata; there is no code to run.

## Connect

One URL exposes only the Data Gleaner tools:

```
https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/sitemap-extractor
```

Two ways to sign in:

- **OAuth (recommended).** Paste the URL alone. Your client opens an Apify sign-in on first use; a free Apify account works.
- **API token.** Send the header `Authorization: Bearer <APIFY_TOKEN>`, with a token from [console.apify.com/settings/integrations](https://console.apify.com/settings/integrations).

Runs are billed to your own Apify account at the prices below. Apify's free plan includes monthly usage credit, which covers a first test.

Want only one tool? Keep just that Actor in the `tools=` list, for example `https://mcp.apify.com/?tools=datagleaner/google-hotels-scraper`.

### Claude Code

```bash
claude mcp add --transport http data-gleaner "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/sitemap-extractor"
```

With a token instead of OAuth, add `--header "Authorization: Bearer $APIFY_TOKEN"`.

### Claude Desktop and Claude.ai

Settings, Connectors, **Add custom connector**: name it `Data Gleaner` and paste the URL. Claude opens the Apify sign-in.

To use a token instead, put this in `claude_desktop_config.json` (it runs the `mcp-remote` bridge, so Node.js must be installed; the header is written without a space after the colon because Claude Desktop on Windows splits arguments at spaces):

```json
{
  "mcpServers": {
    "data-gleaner": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/sitemap-extractor",
        "--header",
        "Authorization:${AUTH_HEADER}"
      ],
      "env": { "AUTH_HEADER": "Bearer <your Apify API token>" }
    }
  }
}
```

### Cursor

`~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "data-gleaner": {
      "url": "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/sitemap-extractor"
    }
  }
}
```

For a token instead of OAuth, add `"headers": { "Authorization": "Bearer ${env:APIFY_TOKEN}" }` next to `url`.

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "data-gleaner": {
      "type": "http",
      "url": "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/sitemap-extractor",
      "headers": { "Authorization": "Bearer ${input:apify-token}" }
    }
  },
  "inputs": [
    { "type": "promptString", "id": "apify-token", "description": "Apify API token", "password": true }
  ]
}
```

Drop the `headers` and `inputs` blocks to use OAuth.

### Other clients

Any client that speaks Streamable HTTP takes the same URL: Windsurf, Cline, ChatGPT connectors, n8n, LangChain, the OpenAI and Anthropic APIs' remote MCP options.

## Tools

Each Actor is one tool, named like `datagleaner--google-hotels-scraper`. Apify's server also adds its helper tools for reading results (`get-actor-run`, `get-dataset-items`, `get-key-value-store-record`, `abort-actor-run`). All five Actors are plain HTTP (no browser), need no login to the target site, and charge only for results returned.

| Tool (Actor) | What it returns | Price |
| --- | --- | --- |
| [Website Contact Details & Email Scraper](https://apify.com/datagleaner/website-contact-details-scraper) | Emails, E.164 phone numbers, social profiles, contact forms and addresses from company websites, each with its source page | $4 per 1,000 websites with contacts found; sites with nothing found are free |
| [Google Hotels Scraper](https://apify.com/datagleaner/google-hotels-scraper) | Hotel prices for exact dates: lowest rate, each booking site's price and link (such as Booking.com, Agoda, Trip.com), ratings, amenities, coordinates | $3 per 1,000 hotels |
| [Weibo Scraper](https://apify.com/datagleaner/weibo-scraper) | Public Weibo posts by keyword or user: full text, dates, likes, reposts, comments, media, poster region, author | $3 per 1,000 posts |
| [Bilibili Scraper](https://apify.com/datagleaner/bilibili-scraper) | Bilibili video search and details (views, likes, coins, tags, uploader), comments with IP location, danmaku | $8 per 1,000 videos, $2 per 1,000 comments, $0.50 per 1,000 danmaku |
| [Sitemap URL Extractor](https://apify.com/datagleaner/sitemap-extractor) | Every page URL of a site from its sitemaps, with lastmod, changefreq and priority | $0.20 per 1,000 URLs |

Each Actor also runs with no input, returning a small built-in example of at most five results, so an agent can see the output shape for a few cents at most.

## What an agent can ask

- "Find the contact email and phone for these 50 company domains."
- "What do hotels in Shinjuku cost for 12 to 14 November, and which booking site is cheapest for each?"
- "Pull the last 200 Weibo posts mentioning Xiaomi SU7 with their engagement."
- "Get the top Bilibili videos for 原神 and their comments."
- "List every blog URL in stripe.com's sitemaps with its last-modified date."

## Files

- `server.json`: the entry for the [official MCP Registry](https://registry.modelcontextprotocol.io) (`io.github.datagleaner/data-gleaner-mcp`).
- `glama.json`: maintainer claim for [Glama](https://glama.ai/mcp/servers).
- `llms-install.md`: setup steps for agents such as Cline that install servers from a README.

## Support

Open an issue here, or use the Issues tab on any Actor's Apify Store page. Data Gleaner is an independent Apify Store developer; Apify runs the MCP server and the billing.

## License

MIT
