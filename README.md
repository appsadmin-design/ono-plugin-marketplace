# Ono Claude Marketplace

Internal [Claude Code](https://code.claude.com) plugin marketplace maintained by **Ono Apps**.

Add this marketplace to Claude Code once, then install any Ono plugin from it.

## Available plugins

| Plugin | Description |
| --- | --- |
| `ono-project-inspector` | Inspects an existing repository and gradually builds a structured, approval-gated AI knowledge base for it (`CLAUDE.md`, `AUDIT.md`, `docs/project/`, and per-topic audit documents), then syncs approved findings back into `CLAUDE.md`. **Never modifies source code.** |

## Install (developers)

> **Local setup for now.** This marketplace and the `ono-project-inspector` plugin are not yet on GitHub. Both repos must be checked out **side by side** under the same parent directory, because the marketplace serves the plugin through a symlink to its sibling repo:
>
> ```
> <parent>/
> ├── ono-claude-marketplace/       ← this repo
> └── Ono-ProjectInspectorPlugin/   ← the plugin repo (sibling)
> ```

### 1. Add the marketplace

From inside Claude Code, point it at your local clone of this repo:

```
/plugin marketplace add ./ono-claude-marketplace
```

Use an absolute path if you are not in the parent directory, e.g.:

```
/plugin marketplace add /Users/you/Developer/AI/ono-claude-marketplace
```

### 2. Install the plugin

```
/plugin install ono-project-inspector@ono-claude-marketplace
```

Claude Code will restart and the plugin's commands, agents, and skills become available.

### 3. Verify

```
/plugin marketplace list
/plugin
```

You should see `ono-claude-marketplace` listed and `ono-project-inspector` installed. The plugin adds the `/inspect`, `/inspect-status`, `/inspect-topic`, and `/inspect-approve` commands.

## Updating

After pulling new changes into either repo:

```
/plugin marketplace update ono-claude-marketplace
```

## How it's wired

- `.claude-plugin/marketplace.json` — the marketplace manifest Claude Code reads. Its single plugin entry uses `"source": "./plugins/ono-project-inspector"`.
- `plugins/ono-project-inspector` — a **symlink** to the sibling `../Ono-ProjectInspectorPlugin` repo. This keeps the plugin's code in its own repository while letting the marketplace serve it with a documented relative `source` path (Claude Code discourages `../` paths in `source`).
- `plugins/ono-project-inspector.json` — a human-readable descriptor of the plugin within this repo.

## Going to GitHub (later)

When the plugin repo is published, drop the symlink and switch the marketplace entry in `.claude-plugin/marketplace.json` to a GitHub source:

```json
{
  "name": "ono-project-inspector",
  "source": { "source": "github", "repo": "onoapps/Ono-ProjectInspectorPlugin" }
}
```

Then developers can add the marketplace directly from GitHub:

```
/plugin marketplace add onoapps/ono-claude-marketplace
```
