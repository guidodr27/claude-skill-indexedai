<!-- v1.0.0 -->
# Skill: /indexedai

Use MCP servers embedded in websites to get richer, structured data instead of plain WebFetch.

## Trigger
- `/indexedai <url> [question]` — query a specific site
- `/indexedai <domain> [question]` — domain-only shorthand
- `/indexedai <question>` — when a URL is implied by context

## Flow obbligatorio

Before fetching anything, tell the user:
> "Checking for MCP server at `<domain>`…"

### Step 1 — Discover MCP via `.well-known`

```
GET https://<DOMAIN>/.well-known/mcp.json
```

**Success** — response contains:
```json
{"mcpServers":{"name":{"url":"https://..."}}}
```
→ extract MCP endpoint URL, proceed to Step 2.

**Failure** (404, timeout >3s, malformed JSON, network error) → skip to **Fallback**.

### Step 2 — Initialize (mandatory handshake)

```
POST <mcp_url>
Content-Type: application/json

{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","clientInfo":{"name":"indexedai-skill","version":"1.0"}}}
```

If response missing `result.capabilities` or status ≥ 400 → skip to **Fallback**.

### Step 3 — List available tools

```
POST <mcp_url>
Content-Type: application/json

{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}
```

Read `result.tools[]`. Pick the tool whose name/description best matches the user's question.
If no tools returned → skip to **Fallback**.

### Step 4 — Call the best tool

```
POST <mcp_url>
Content-Type: application/json

{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"<TOOL_NAME>","arguments":{...}}}
```

Map user's question to tool arguments. Return `result.content`.

### Fallback

Tell the user:
> "No MCP server found at `<domain>`, using WebFetch."

Then use `WebFetch` on the most relevant URL for the question.

## Multiple domains

If query contains multiple domains, run Step 1 for **all** domains in parallel before calling any tool.
Then call each MCP concurrently.

## Output format

Always prefix answers with source:
- `[MCP: domain.com]` — data from site's own MCP server
- `[Web: domain.com]` — fallback WebFetch

Example:
```
[MCP: docs.stripe.com]
Webhooks retry up to 3 days with exponential backoff...
```

## Implementation notes

- HTTP stateless — no persistent connection to maintain
- No SDK or library needed — plain JSON-RPC over HTTP POST
- If `initialize` succeeds but `tools/list` fails → Fallback
- Never invent tool names — only use names returned by `tools/list`
- Respect `result.content[].type`: `text` = display as-is, `resource` = fetch if needed
