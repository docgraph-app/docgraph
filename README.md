# DocGraph

**DocGraph is a free, privacy-first, AI-native personal knowledge graph for Mac** —
a notes app where your HTML and Markdown notes, the links between them and an AI
that has read all of them live in one place. Your notes stay plain files in a
folder you own, and the AI runs on your Mac by default: no account, no
subscription, no telemetry.

**[⬇ Download for Mac (Apple Silicon)](https://docgraph.app/download)** ·
[Website](https://docgraph.app) · [Documentation](https://docs.docgraph.app) ·
[FAQ](https://docgraph.app/faq)

- **Ask your notes.** Chat with your whole vault, a folder, a tag or a date range;
  answers cite the notes they came from. A built-in local model means every
  question is free and works offline. Ollama, Apple's on-device model and cloud
  models (OpenAI, Anthropic, OpenRouter) work too, if you choose.
- **Plain files.** `.html` and `.md` notes in a normal folder — open an existing
  Obsidian vault in place, wikilinks and callouts included. PDFs, Word files,
  spreadsheets and scans are searchable alongside them.
- **Knowledge tools.** Backlinks, graph view, tables and saved queries, tasks,
  calendar, whiteboards and Excalidraw sketches.
- **Private by design.** Deny-by-default network gate with a visible activity
  log, per-file encryption, secret detection, Git version history.
- **Works with your agents.** A built-in MCP server lets Claude Code, Codex,
  Cursor and others use your vault (off by default; every write asks first).

Free to use. This repository hosts the downloads, the auto-update manifest and
the public issue tracker; the application source is not public.

---

## Install

Requires a Mac with Apple Silicon (M1 or later) and macOS 13.3 Ventura or later.

1. Download `DocGraph-mac-apple-silicon.dmg` from
   [docgraph.app/download](https://docgraph.app/download) or the
   [latest release](https://github.com/docgraph-app/docgraph/releases/latest).
2. Open it and drag DocGraph to Applications.

DocGraph is signed with a Developer ID and notarized by Apple, so it opens like
any other Mac app. Intel Macs, Windows and Linux are planned for later releases.

## Updating

Check whenever you like: **Settings → General → Updates → Check for updates**.

DocGraph also checks on its own about once a day (a few minutes after launch,
then roughly every 24 hours). This check is always on, so security fixes reach
everyone; **Skip this version** hides an update you don't want.

A check fetches one small file from this repository and **sends nothing about
you**: not your version, not your platform, not any identifier. There is no
account, no telemetry and no install id to send. Every check shows on the
network badge in the title bar and is recorded in your Activity log.

**Nothing installs by itself.** When a newer version exists you get a bar
offering it, with the release notes, and the choice to update now, be reminded
later, or skip that version. Update bundles are verified against a signing key
embedded in the app, so a tampered or substituted download is rejected.

Every release is listed at
[docs.docgraph.app/changelog](https://docs.docgraph.app/changelog).

## Use DocGraph from Claude Code

This repository is also a Claude Code plugin marketplace. The `docgraph`
plugin connects Claude Code to DocGraph's local MCP server and adds a
`/docgraph` skill that draws ERDs, architecture and flow diagrams on a
whiteboard, or reads and writes your notes:

```
/docgraph create an ERD for the database in ~/work/shop
```

1. In DocGraph: **Settings → Connections → Agents & MCP**, enable the server and copy the token.
2. `export DOCGRAPH_MCP_TOKEN="<token>"` in your shell profile.
3. In Claude Code: `/plugin marketplace add docgraph-app/docgraph`, then
   `/plugin install docgraph@docgraph`, and restart Claude Code.

Every write the agent makes is confirmed inside DocGraph first. Details in
[docgraph/README.md](docgraph/README.md) and at
[docs.docgraph.app/ai/mcp](https://docs.docgraph.app/ai/mcp/).

## Plugins

Third-party plugins are **coming in a later release**. The graph, Vault Map and
Problems panels are built in today.

The plugin SDK ([plugin-api](https://github.com/docgraph-app/plugin-api)), the
[registry](https://github.com/docgraph-app/registry) and an example plugin
([word-count](https://github.com/docgraph-app/word-count)) are already open
source (MIT) for anyone who wants an early look, but the current app does not
load plugins from them yet.

## Known limitations

- **Intel Macs (x86_64) are not built.** The embedding runtime DocGraph uses
  (ONNX Runtime via `ort`) publishes no prebuilt binary for that target, so the
  build cannot be produced without compiling it from source.

## Report a bug or request a feature

The source lives elsewhere, but bugs and ideas are tracked here in the open.

- **Bug:** in DocGraph, choose **Help → Report a Problem…** (or **Settings → General → Updates →
  Report a problem…**). It opens a
  [bug report](https://github.com/docgraph-app/docgraph/issues/new?template=bug_report.yml)
  with your version and system already filled in. Nothing is sent until you
  submit it yourself.
- **Feature request:** [open one here](https://github.com/docgraph-app/docgraph/issues/new?template=feature_request.yml).
  Check the [existing issues](https://github.com/docgraph-app/docgraph/issues)
  first; a 👍 on one that's already there counts.
- **Question:** ask in [Discussions](https://github.com/docgraph-app/docgraph/discussions).
- **Private or no GitHub account:** email **support@docgraph.app**.
- **Security problem:** see [SECURITY.md](SECURITY.md). Please don't post it publicly.

## Licence

DocGraph is **free to use** but is not open source. Downloads here are covered
by the licence included with the app. The plugin SDK, registry, and example
plugins are MIT.
