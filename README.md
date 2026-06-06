# indexedai — Claude Code Skill

> Query websites through their own MCP servers instead of plain WebFetch — getting structured, semantic data straight from the source.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)

---

## What it does

Many websites expose an [MCP](https://modelcontextprotocol.io) server at `/.well-known/mcp.json`.
`/indexedai` discovers and uses that server automatically — so instead of scraping HTML, Claude calls the site's own API and gets back clean, structured answers.

```
User: /indexedai docs.stripe.com how does webhook retry work?

Claude: Checking for MCP server at docs.stripe.com…
        [MCP: docs.stripe.com]
        Stripe retries failed webhook deliveries up to 3 days
        with exponential backoff starting at 1 hour...
```

Falls back to plain WebFetch when no MCP server is found.

## How it works

```
/indexedai <domain> <question>
       │
       ▼
GET /.well-known/mcp.json
       │
   ┌───┴────────────────┐
   │ MCP found          │ No MCP / error
   ▼                    ▼
Initialize           WebFetch fallback
List tools           [Web: domain.com]
Call best tool
[MCP: domain.com]
```

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

# Multiple sites (queried in parallel)
/indexedai stripe.com and docs.github.com webhook events
```

---

## Output format

| Prefix | Meaning |
|--------|---------|
| `[MCP: domain.com]` | Data from the site's own MCP server |
| `[Web: domain.com]` | Fallback — plain WebFetch |

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
