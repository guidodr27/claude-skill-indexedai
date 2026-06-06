# indexedai — Claude Code Skill

> Query any website via MCP — structured data, zero context overhead.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![IndexedAI](https://img.shields.io/badge/Powered%20by-IndexedAI-orange)](https://drivecode.it)

---

## The problem with normal MCP servers

When you add an MCP server to Claude, it loads **every tool definition at startup** — filling your context window before you've typed a single message.

`/indexedai` works differently. It loads MCP **on demand**, only when you invoke it. No idle overhead, no wasted tokens.

---

## What it does

`/indexedai` queries any website through [MCP (Model Context Protocol)](https://modelcontextprotocol.io) and returns clean, structured data — not raw HTML.

It works with sites that expose a `/.well-known/mcp.json` discovery endpoint, including sites indexed by **[IndexedAI.tech](https://drivecode.it)** — a service that generates hosted MCP endpoints and `llms.txt` files for any website.

If no MCP server is found, it falls back to plain WebFetch automatically.

```
User: /indexedai docs.stripe.com how does webhook retry work?

Claude: Checking for MCP server at docs.stripe.com…
        [MCP: docs.stripe.com]
        Stripe retries failed webhook deliveries up to 3 days
        with exponential backoff starting at 1 hour...
```

---

## Key benefits

| Benefit | Detail |
|---------|--------|
| **Zero context overhead** | MCP loads only when invoked — never at startup |
| **Structured data** | Semantic content via MCP, not raw HTML scraping |
| **Always returns an answer** | Graceful WebFetch fallback when MCP is unavailable |
| **Parallel queries** | Multiple domains discovered and queried concurrently |

---

## How it works

```
/indexedai <domain> <question>
       │
       ▼
GET /.well-known/mcp.json
       │
   ┌───┴────────────────────┐
   │ MCP endpoint found     │ No MCP / error
   ▼                        ▼
Initialize (JSON-RPC)    WebFetch fallback
List tools               [Web: domain.com]
Call best-matching tool
[MCP: domain.com]
```

Discovery happens via [`.well-known/mcp.json`](https://modelcontextprotocol.io) — the MCP standard for advertising server endpoints. If a site doesn't have one natively, [IndexedAI.tech](https://drivecode.it) can generate one.

---

## Install

**One command:**

```bash
mkdir -p ~/.claude/skills/indexedai && \
  curl -fsSL https://raw.githubusercontent.com/guidodr27/claude-skill-indexedai/main/SKILL.md \
  > ~/.claude/skills/indexedai/SKILL.md
```

**Manual:**
1. Download `SKILL.md` from this repo
2. Place it at `~/.claude/skills/indexedai/SKILL.md`
3. Restart Claude Code (or reload skills)

---

## Usage

```
# Full URL with question
/indexedai https://docs.stripe.com "webhook retry policy"

# Domain shorthand
/indexedai openai.com rate limits

# Multiple sites queried in parallel
/indexedai stripe.com and docs.github.com webhook events
```

---

## Output format

| Prefix | Meaning |
|--------|---------|
| `[MCP: domain.com]` | Structured data via MCP endpoint |
| `[Web: domain.com]` | Fallback — plain WebFetch |

---

## Want MCP for your site?

[IndexedAI.tech](https://drivecode.it) analyzes any website and generates:
- A hosted **MCP server endpoint** (queryable via this skill)
- A custom **`llms.txt`** file for AI agent readability
- An **Agent Readiness Score** across 5 dimensions
- **Analytics** — see which AI systems (Claude, GPT, Perplexity, Cursor…) access your site, how often, and what they query

---

## Requirements

- [Claude Code](https://claude.ai/code) (any version supporting skills)
- No additional dependencies

---

## Examples

See the [`examples/`](examples/) directory for real-world usage walkthroughs.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

[MIT](LICENSE) © 2026 guidodr27
