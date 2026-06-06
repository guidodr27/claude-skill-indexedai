# Changelog

All notable changes to this project will be documented here.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Versioning: [SemVer](https://semver.org/)

---

## [1.0.0] — 2026-06-05

### Added
- Initial public release
- MCP discovery via `/.well-known/mcp.json`
- Mandatory JSON-RPC handshake before tool calls
- Automatic tool selection based on user question
- WebFetch fallback when no MCP found
- Parallel discovery for multi-domain queries
- Source attribution: `[MCP: domain]` / `[Web: domain]`
- User-facing status messages during discovery
- Timeout hint: skip MCP if `.well-known` takes >3s
