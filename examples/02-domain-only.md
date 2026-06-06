# Example 2 — Domain shorthand

## Command

```
/indexedai openai.com rate limits
```

## Discovery phase

```
Checking for MCP server at openai.com…
→ GET https://openai.com/.well-known/mcp.json  404
No MCP server found at openai.com, using WebFetch.
→ WebFetch https://openai.com/pricing (most relevant page)
```

## Expected output

```
[Web: openai.com]
GPT-4o rate limits vary by tier. Free tier: 3 RPM / 200 RPD.
Tier 1 ($5 spent): 500 RPM / unlimited RPD...
```

## Notes

- `https://` prefix is optional — skill infers it
- No MCP found → clean fallback message, then WebFetch
- Source tag changes to `[Web: openai.com]`
