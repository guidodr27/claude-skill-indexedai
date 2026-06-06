# Contributing

Contributions welcome — bug reports, new examples, SKILL.md improvements.

## Test a change locally

```bash
cp SKILL.md ~/.claude/skills/indexedai/SKILL.md
```

Then in Claude Code:
```
/indexedai indexedai.tech what is this?
```

Confirm output starts with `[MCP: indexedai.tech]` (not `[Web:]`).

Test fallback:
```
/indexedai example.com test
```

Confirm output starts with `[Web: example.com]`.

## What makes a good bug report

- Domain that has a `.well-known/mcp.json` but the skill fails
- Paste the raw JSON from `GET https://domain/.well-known/mcp.json`
- What Claude said vs. what you expected

## PR checklist

- [ ] Tested with at least one real site that has an MCP server
- [ ] Tested fallback with a site that has no MCP server
- [ ] Updated `CHANGELOG.md` under `[Unreleased]`
- [ ] `SKILL.md` still passes CI checks (`grep -q "## Trigger" SKILL.md`)

## Versioning

This project follows SemVer:
- **Patch** — wording fixes, edge case handling that doesn't change behavior
- **Minor** — new capabilities (new output formats, new discovery methods)
- **Major** — breaking changes to the skill trigger or output format
