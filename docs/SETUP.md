# Setup — Give This to Your Claude Code

Copy-paste the prompt below into your Claude Code session. It will set up all the MCP tools the swarm needs.

---

## The Prompt

```
Set up the following MCP servers for this project. Install and configure each one — if it's not already available, find the package and wire it into .mcp.json:

1. **long-term-memory** — persistent knowledge store across sessions (FastMCP, sqlite-backed)
2. **sequential-thinking** — multi-step reasoning for planning and triage (FastMCP)
3. **duckduckgo** — web search for research chains
4. **fetch** — HTTP content fetching for external resources
5. **context7** — library/framework documentation lookup
6. **claude-in-chrome** — Chrome browser automation (extension-based, primary for frontend verification)
7. **playwright** — headless browser automation (fallback when Chrome extension unavailable)

After setup, verify each one works by calling a simple test query on each tool. Report which ones are online.
```

---

## What Each Tool Does in the Swarm

| Tool | Role | Used By |
|------|------|---------|
| long-term-memory | Shared brain — findings, root causes, domain knowledge persist across agents and sessions | All agents |
| sequential-thinking | Deep reasoning for plans, triage decisions, debugging | Jarvis, managers |
| duckduckgo + fetch | Research chain: LTM → WebSearch → DuckDuckGo → context7 | All agents |
| context7 | Framework/library docs lookup before coding | Managers, sub-agents |
| claude-in-chrome | PRIMARY browser automation — screenshots, click-through, visual verification | Jarvis, Fable, managers |
| playwright | FALLBACK browser automation — headless, Jarvis-only | Jarvis |

## After Setup

Read `docs/CLAUDE.md` — that's the full operating system rulebook. Start with `go` or `swarm` to begin.
