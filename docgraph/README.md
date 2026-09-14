# DocGraph for Claude Code

A Claude Code plugin that connects to DocGraph's local MCP server and adds a
`/docgraph` skill, so you can type:

```
/docgraph create an ERD for the database in /Users/me/work/shop
/docgraph draw the architecture of this repo as Diagrams/shop-architecture.wb
/docgraph add a payments table linked to orders on Diagrams/shop-erd.wb
```

Plugin skills are namespaced, so `/docgraph:docgraph …` is the fully qualified
form if the short name is ever ambiguous.

Plain prose works too once the plugin is installed — "use DocGraph to draw an
ERD of my database" — the skill just makes the intent explicit.

## Install

1. In DocGraph: **Settings → AI → MCP**, enable the server, **Copy** the token.
2. Put the token in your shell profile:
   ```bash
   export DOCGRAPH_MCP_TOKEN="<paste>"
   ```
3. In Claude Code:
   ```
   /plugin marketplace add docgraph-app/docgraph
   /plugin install docgraph@docgraph
   ```
   Restart Claude Code so it picks up the environment variable.

Every write the agent makes is confirmed inside DocGraph before it lands.

## Layout

```
.claude-plugin/marketplace.json   ← marketplace manifest (repo root)
docgraph/
  .claude-plugin/plugin.json      ← plugin manifest
  .mcp.json                       ← DocGraph MCP server (token from $DOCGRAPH_MCP_TOKEN)
  skills/docgraph/SKILL.md        ← the /docgraph skill
```

The source of truth is `integrations/claude-code/` in the app repo; it is
copied to the public `docgraph-app/docgraph` repo on release. The node/edge
vocabulary in SKILL.md mirrors `ai/tools/board.rs` — update both together.
