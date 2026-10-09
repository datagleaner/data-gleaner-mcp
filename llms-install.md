# Installing the Data Gleaner MCP server

This is a remote server. Nothing is cloned, built or run locally.

1. Add a Streamable HTTP server named `data-gleaner` with this URL:
   `https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/sitemap-extractor`
2. Authentication, pick one:
   - If the client supports MCP OAuth, add nothing; the user signs in to Apify in the browser on first use.
   - Otherwise ask the user for an Apify API token (console.apify.com/settings/integrations) and add the header `Authorization: Bearer <token>`.
3. Cline `cline_mcp_settings.json` example with a token:

```json
{
  "mcpServers": {
    "data-gleaner": {
      "type": "streamableHttp",
      "url": "https://mcp.apify.com/?tools=datagleaner/website-contact-details-scraper,datagleaner/google-hotels-scraper,datagleaner/weibo-scraper,datagleaner/bilibili-scraper,datagleaner/sitemap-extractor",
      "headers": { "Authorization": "Bearer <APIFY_TOKEN>" }
    }
  }
}
```

4. Check: list the server's tools. Five Data Gleaner Actor tools should appear (`datagleaner--website-contact-details-scraper`, `datagleaner--google-hotels-scraper`, `datagleaner--weibo-scraper`, `datagleaner--bilibili-scraper`, `datagleaner--sitemap-extractor`), plus Apify's helper tools for reading results, such as `get-dataset-items`. Without a valid token or OAuth sign-in the server answers HTTP 401.
