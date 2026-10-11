# Data Gleaner MCP server

Fifteen web data tools for AI agents, served as one remote MCP server. Contacts and lead generation: company website emails, company enrichment by domain, team pages, YouTube channel emails, link-in-bio emails, app and Shopify app developer emails, Shopify agencies, researcher emails, YC founders and newly funded companies from SEC Form D. Travel: Google Hotels prices. China social: Weibo posts, Bilibili videos and comments. Site and web: sitemap URLs. You pay per result returned, nothing to install and nothing to host.

The tools are [Data Gleaner](https://apify.com/datagleaner) Actors on Apify, served through Apify's official hosted MCP server at `mcp.apify.com`. This repository holds the connection configs and the registry metadata; there is no code to run.

## Connect

One URL exposes only the Data Gleaner tools:

```
https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/team-page-contacts,datagleaner/app-developer-email-finder,datagleaner/shopify-partner-contacts,datagleaner/researcher-email-finder,datagleaner/youtube-channel-contacts,datagleaner/sitemap-extractor,datagleaner/form-d-funding-leads,datagleaner/yc-founder-contacts,datagleaner/bio-link-email-finder,datagleaner/company-enrichment-by-domain,datagleaner/shopify-app-developer-contacts
```

Two ways to sign in:

- **OAuth (recommended).** Paste the URL alone. Your client opens an Apify sign-in on first use; a free Apify account works.
- **API token.** Send the header `Authorization: Bearer <APIFY_TOKEN>`, with a token from [console.apify.com/settings/integrations](https://console.apify.com/settings/integrations).

Runs are billed to your own Apify account at the prices below. Apify's free plan includes monthly usage credit, which covers a first test.

Want only one tool? Keep just that Actor in the `tools=` list, for example `https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/team-page-contacts,datagleaner/app-developer-email-finder,datagleaner/shopify-partner-contacts,datagleaner/researcher-email-finder,datagleaner/youtube-channel-contacts,datagleaner/sitemap-extractor,datagleaner/form-d-funding-leads,datagleaner/yc-founder-contacts,datagleaner/bio-link-email-finder,datagleaner/company-enrichment-by-domain,datagleaner/shopify-app-developer-contacts`.

### Claude Code

```bash
claude mcp add --transport http data-gleaner "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/team-page-contacts,datagleaner/app-developer-email-finder,datagleaner/shopify-partner-contacts,datagleaner/researcher-email-finder,datagleaner/youtube-channel-contacts,datagleaner/sitemap-extractor,datagleaner/form-d-funding-leads,datagleaner/yc-founder-contacts,datagleaner/bio-link-email-finder,datagleaner/company-enrichment-by-domain,datagleaner/shopify-app-developer-contacts"
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
        "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/team-page-contacts,datagleaner/app-developer-email-finder,datagleaner/shopify-partner-contacts,datagleaner/researcher-email-finder,datagleaner/youtube-channel-contacts,datagleaner/sitemap-extractor,datagleaner/form-d-funding-leads,datagleaner/yc-founder-contacts,datagleaner/bio-link-email-finder,datagleaner/company-enrichment-by-domain,datagleaner/shopify-app-developer-contacts",
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
      "url": "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/team-page-contacts,datagleaner/app-developer-email-finder,datagleaner/shopify-partner-contacts,datagleaner/researcher-email-finder,datagleaner/youtube-channel-contacts,datagleaner/sitemap-extractor,datagleaner/form-d-funding-leads,datagleaner/yc-founder-contacts,datagleaner/bio-link-email-finder,datagleaner/company-enrichment-by-domain,datagleaner/shopify-app-developer-contacts"
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
      "url": "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/team-page-contacts,datagleaner/app-developer-email-finder,datagleaner/shopify-partner-contacts,datagleaner/researcher-email-finder,datagleaner/youtube-channel-contacts,datagleaner/sitemap-extractor,datagleaner/form-d-funding-leads,datagleaner/yc-founder-contacts,datagleaner/bio-link-email-finder,datagleaner/company-enrichment-by-domain,datagleaner/shopify-app-developer-contacts",
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

Each Actor is one tool, named like `datagleaner--google-hotels-scraper`. Apify's server also adds its helper tools for reading results (`get-actor-run`, `get-dataset-items`, `get-key-value-store-record`, `abort-actor-run`). None of the fifteen needs a login to the target site, and all charge only for results returned.

| Tool (Actor) | What it returns | Price |
| --- | --- | --- |
| [Company Contact Details & Website Email Finder](https://apify.com/datagleaner/website-contact-details-scraper) | Emails, E.164 phone numbers, social profiles, contact forms and addresses from company websites, each with its source page | $2 per 1,000 websites where an email is found; every other site is free |
| [Company Enrichment by Domain](https://apify.com/datagleaner/company-enrichment-by-domain) | Name, description, logo, official social profiles (LinkedIn, X, Facebook, Instagram, YouTube, TikTok, GitHub), email, phone, address and founding date for each domain; a Clearbit alternative | $0.80 per 1,000 enriched companies; misses are free |
| [Company Leadership & Team Page Scraper](https://apify.com/datagleaner/team-page-contacts) | Named people from a company's own team, about, leadership and Impressum pages, with title, seniority, LinkedIn and published email; never guesses emails | $0.002 per person |
| [YouTube Channel Contacts](https://apify.com/datagleaner/youtube-channel-contacts) | Public emails of YouTube channels from the description, About links, link-in-bio page and creator website, plus socials, subscribers and country; search by channel or niche keyword | $0.015 per channel with an email; channels without one are free |
| [Link in Bio Email Finder](https://apify.com/datagleaner/bio-link-email-finder) | A creator's email, phone and socials from a Linktree, Taplink, AllMyLinks or similar page, plus Instagram, TikTok, YouTube, X, booking and shop links | $0.004 per profile with an email; others are free |
| [App Developer Email Finder](https://apify.com/datagleaner/app-developer-email-finder) | Developer email, website, address, phone and privacy policy of apps on Google Play, the App Store, Steam and the Chrome Web Store | $0.0015 per app with an email; apps without one are free |
| [Shopify App Developer Contacts](https://apify.com/datagleaner/shopify-app-developer-contacts) | Shopify App Store apps with the developer's support email, website and address, plus rating and reviews; search by keyword, category or URL | $0.002 per app with an email; others are free |
| [Shopify Partner & Agency Contacts](https://apify.com/datagleaner/shopify-partner-contacts) | Agencies and experts from the Shopify Partner Directory by country and service, with public email, phone, website, rating and tier | $0.005 per partner with a contact; others are free |
| [Researcher Email Finder](https://apify.com/datagleaner/researcher-email-finder) | Researchers by topic with the email they published in PubMed papers, plus affiliation, ORCID and recent papers | $0.005 per researcher with an email |
| [YC Founder Contacts](https://apify.com/datagleaner/yc-founder-contacts) | Y Combinator founders by company, batch or industry: names, titles, LinkedIn, X, company website and emails | $0.003 per company with founders; others are free |
| [Form D Funding Leads](https://apify.com/datagleaner/form-d-funding-leads) | Companies that just raised money, from SEC Form D filings: amount raised, phone, address and named executives; filter by keyword, state, industry or amount | $0.01 per filing with executives |
| [Google Hotels Scraper](https://apify.com/datagleaner/google-hotels-scraper) | Hotel prices for exact dates: lowest rate, each booking site's price and link (such as Booking.com, Agoda, Trip.com), ratings, amenities, coordinates | $3 per 1,000 hotels |
| [Weibo Scraper](https://apify.com/datagleaner/weibo-scraper) | Public Weibo posts by keyword or user: full text, dates, likes, reposts, comments, media, poster region, author | $3 per 1,000 posts |
| [Bilibili Scraper](https://apify.com/datagleaner/bilibili-scraper) | Bilibili video search and details (views, likes, coins, tags, uploader), comments with IP location, danmaku | $8 per 1,000 videos, $2 per 1,000 comments, $0.50 per 1,000 danmaku |
| [Sitemap URL Extractor](https://apify.com/datagleaner/sitemap-extractor) | Every page URL of a site from its sitemaps, with lastmod, changefreq and priority | $0.20 per 1,000 URLs |

Each Actor also runs with no input, returning a small built-in example of at most five results, so an agent can see the output shape for a few cents at most.

## What an agent can ask

- "Enrich these 200 domains with LinkedIn pages, emails and phones."
- "Which startups filed a Form D this week, and who are the named executives?"
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
