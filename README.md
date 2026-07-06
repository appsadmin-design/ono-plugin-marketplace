# Ono Claude Marketplace

Internal [Claude Code](https://code.claude.com) plugin marketplace maintained by **Ono Apps**.

Add this marketplace to Claude Code once, then install any Ono plugin from it.

## Available plugins

| Plugin | Description |
| --- | --- |
| `ono-project-inspector` | Inspects an existing repository and gradually builds a structured, approval-gated AI knowledge base for it (`CLAUDE.md`, `AUDIT.md`, `docs/project/`, and per-topic audit documents), then syncs approved findings back into `CLAUDE.md`. **Never modifies source code.** |
| `ono-react-native-dev-plugin` | React Native SDLC workflow for Ono Apps: feature analysis, dev planning, implementation, code review, debugging, QA handoff, and release readiness. |
| `ono-plugin-QA-ReactNative` | Figma-grounded QA test planning and dev/QA coverage gap analysis for Ono Apps' React Native features, run in parallel with `ono-react-native-dev-plugin`. |

## Install (developers)

Nothing to clone. The marketplace and each plugin are fetched directly from GitHub.

### 1. Add the marketplace

```
/plugin marketplace add appsadmin-design/ono-plugin-marketplace
```

### 2. Install the plugin

```
/plugin install ono-project-inspector@ono-plugin-marketplace
```

Claude Code restarts and the plugin's commands, agents, and skills become available. The plugin adds the `/inspect`, `/inspect-status`, `/inspect-topic`, and `/inspect-approve` commands.

### 3. Verify

```
/plugin marketplace list
/plugin
```

You should see `ono-plugin-marketplace` listed and `ono-project-inspector` installed.

## Updating

After new changes are published to the plugin or marketplace repo:

```
/plugin marketplace update ono-plugin-marketplace
```

## How it's wired

- `.claude-plugin/marketplace.json` — the marketplace manifest. Its `ono-project-inspector` entry uses an **HTTPS `url` source** pointing at the plugin's own repository, `https://github.com/appsadmin-design/ono-plugin-project-inspector.git`. HTTPS is used (rather than a `github` source, which clones over SSH) so installs work on any machine without SSH keys configured.
- The plugin's code is **not** duplicated or symlinked into this repo. It lives only in its own repository; Claude Code fetches it from GitHub on install.

The source pins `ref: main`, so the marketplace tracks the plugin repo's default branch. To pin installs to a specific release instead, change `ref` to a tag (or add a `sha`):

```json
{
  "name": "ono-project-inspector",
  "source": {
    "source": "url",
    "url": "https://github.com/appsadmin-design/ono-plugin-project-inspector.git",
    "ref": "v0.7.0"
  }
}
```
