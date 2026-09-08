# DocGraph

A local-first knowledge base for your own HTML and Markdown files. Your notes
stay on your disk, in formats you already own — DocGraph reads the folder you
point it at and never uploads it anywhere.

**[⬇ Download the latest release](https://github.com/docgraph-app/docgraph/releases/latest)**

Free to use. This repository hosts the downloads and the auto-update manifest;
the application source is not public. The **plugin ecosystem is open source**
(MIT) — see below.

---

## Install

Pick the file for your system from the
[latest release](https://github.com/docgraph-app/docgraph/releases/latest).

| System | File |
|---|---|
| macOS (Apple Silicon) | `DocGraph_*_aarch64.dmg` |
| Windows | `DocGraph_*_x64-setup.exe` (or the `.msi`) |
| Linux (Debian/Ubuntu) | `DocGraph_*_amd64.deb` |
| Linux (Fedora/RHEL) | `DocGraph-*.x86_64.rpm` |
| Linux (any) | `DocGraph_*_amd64.AppImage` |

Intel Macs are not currently built — see [Known limitations](#known-limitations).

### First launch: your OS may show a warning

DocGraph is signed for **auto-updates** — every download is cryptographically
verified before it installs. On first launch your system may still ask you to
confirm. Here is how, on each platform.

**macOS** — "DocGraph can't be opened because it is from an unidentified
developer", or "damaged and can't be opened".

1. Open the `.dmg` and drag DocGraph to Applications.
2. In Applications, **right-click** (or Control-click) DocGraph → **Open**.
3. Click **Open** in the dialog.

You only do this once. If macOS insists the app is "damaged", it has quarantined
it; clear that flag:

```bash
xattr -dr com.apple.quarantine /Applications/DocGraph.app
```

**Windows** — "Windows protected your PC" (SmartScreen).

1. Click **More info**.
2. Click **Run anyway**.

**Linux** — no signing gate. For the AppImage, make it executable first:

```bash
chmod +x DocGraph_*_amd64.AppImage && ./DocGraph_*_amd64.AppImage
```

## Updating

Check whenever you like: **Settings → Updates → Check for updates**.

You can also let DocGraph look on its own. **Settings → Updates → Check for
updates automatically** is off until you switch it on, and installing an update
never switches it on for you. With it on, DocGraph checks a few minutes after
you open it, and about once a day if you leave it running — at most once a day
however often you open the app.

A check fetches one small file from this repository and **sends nothing about
you**: not your version, not your platform, not any identifier. There is no
account, no telemetry and no install id to send. Every check turns on the
network indicator in the title bar while it happens and is recorded in your
Activity log, marked so you can tell an automatic check from one you asked for.

**Nothing installs by itself.** When a newer version exists you get a bar
offering it, with the release notes, and the choice to update now, be reminded
later, or skip that version — skipping one release still lets the next one
through. Update bundles are verified against a signing key embedded in the app,
so a tampered or substituted download is rejected.

Release notes for the version you are running are in the app under **Settings →
Updates → What's new in this version**, and the notes for every release are on
that release's page here.

## Write a plugin

The plugin API is open source (MIT) and anyone can publish to the in-app store.

- **[plugin-api](https://github.com/docgraph-app/plugin-api)** — the typed SDK
- **[registry](https://github.com/docgraph-app/registry)** — publish by opening a PR
- **[word-count](https://github.com/docgraph-app/word-count)** — a complete example in ~100 lines

Plugins are yours: you keep the copyright and choose your own licence.

## Known limitations

- **Intel Macs (x86_64) are not built.** The embedding runtime DocGraph uses
  (ONNX Runtime via `ort`) publishes no prebuilt binary for that target, so the
  build cannot be produced without compiling it from source.
- **OS code-signing is not yet provisioned**, so first launch needs the
  one-time confirmation described under [Install](#install).

## Issues

Bug reports and feature requests are welcome in this repository's
[issues](https://github.com/docgraph-app/docgraph/issues), even though the
source lives elsewhere.

## Licence

DocGraph is **free to use** but is not open source. Downloads here are covered
by the licence included with the app. The plugin SDK, registry, and example
plugins are MIT.
