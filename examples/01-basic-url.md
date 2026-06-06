# Example 1 — Full URL with question

## Command

```
/indexedai https://docs.stripe.com "how does webhook retry work?"
```

## Discovery phase

```
Checking for MCP server at docs.stripe.com…
→ GET https://docs.stripe.com/.well-known/mcp.json  ✓
→ POST /mcp  initialize  ✓
→ POST /mcp  tools/list  → ["search", "get_page", ...]
→ POST /mcp  tools/call  search("webhook retry")
```

## Expected output

```
[MCP: docs.stripe.com]
Stripe retries failed webhook deliveries for up to 3 days using
exponential backoff. The first retry occurs 1 hour after failure,
then 6h, 12h, 24h, 48h, and 72h...
```

## Notes

- Source tag `[MCP: docs.stripe.com]` confirms MCP was used
- Question passed as argument to the most relevant tool
- No HTML scraping involved
