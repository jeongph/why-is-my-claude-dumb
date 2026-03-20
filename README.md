# why-is-my-claude-dumb

> Why is my Claude so dumb? Let's find out.

Your Claude Code environment might be missing plugins that could make it significantly smarter. This plugin analyzes your local environment, codebase, and current setup, then recommends official marketplace plugins tailored to your workflow.

## Installation

**1. Add the marketplace**

```
/plugin marketplace add jeongph/why-is-my-claude-dumb
```

**2. Install the plugin**

```
/plugin install why-is-my-claude-dumb@jeongph-why-is-my-claude-dumb
```

**3. Run it**

```
/why-is-my-claude-dumb:why-dumb
```

## How it works

### Full analysis (`/why-dumb`)

Walks you through a guided analysis:

1. Asks about your workflow and pain points
2. Scans your environment (OS, languages, installed plugins, Claude settings, MCP servers)
3. Reads the official marketplace catalog (80+ plugins)
4. Recommends what's missing with clear explanations
5. Installs selected plugins on the spot

### Auto-trigger (dumb-doctor)

When you express frustration with Claude's performance, the `dumb-doctor` agent automatically offers to run an environment check and suggest 1-2 high-impact plugins.

## What gets analyzed

| Category | Details |
|----------|---------|
| System | OS, platform, architecture |
| Languages | Installed runtimes (Node, Python, Java, Go, Rust, etc.) |
| Plugins | Currently installed plugins vs. available catalog |
| Configuration | Claude Code settings, hooks, permissions |
| Integrations | Connected MCP servers |
| Project context | Project type, CLAUDE.md presence (when run inside a repo) |

## Examples

```
/why-is-my-claude-dumb:why-dumb

🔍 Environment Analysis
━━━━━━━━━━━━━━━━━━━━━━
OS: Linux x86_64 | Languages: Java 17, Node 20, Python 3.11
Project: Spring Boot | Installed plugins: 3

💡 These plugins would make a difference
━━━━━━━━━━━━━━━━━━━━━━
1. jdtls-lsp (Java Language Server)
   Catches type errors and missing imports in real-time as you edit.
   Right now you only see errors after building.

2. claude-md-management (Project Context Manager)
   Creates and maintains a CLAUDE.md project description.
   Without it, Claude has to re-learn your project every session.

👉 Tell me which ones to install! (e.g. "install 1 and 2")
```

## License

[MIT](LICENSE)
