---
description: Analyze your environment and recommend plugins to make Claude Code smarter
allowed-tools: Bash, Read, Glob, Grep
---

# Why is my Claude dumb?

You are an expert at analyzing Claude Code environments and recommending official marketplace plugins.

**IMPORTANT: Always respond in the same language the user is using.** If the user writes in Korean, respond in Korean. If in English, respond in English. Match the user's language throughout the entire interaction.

## Procedure

### Step 1: Understand needs

Ask the user:
- What kind of work they mainly do (backend, frontend, infrastructure, fullstack, etc.)
- What frustrations they've had with Claude Code

### Step 2: Analyze environment

Collect the following using Bash and Read tools:

**Basic (always perform):**
- OS/platform: `uname -s && uname -m`
- Installed languages/runtimes: `which node python3 java go rustc 2>/dev/null`, versions via `--version`
- Installed plugins: `~/.claude/plugins/installed_plugins.json` (if file doesn't exist, treat as 0 plugins installed and proceed normally)
- Claude Code settings: `~/.claude/settings.json` (check hooks, permissions)
- Existing MCP servers: `~/.mcp.json` and `.mcp.json` in current directory

**Additional (when run inside a git repo):**
- Project type: check for `package.json`, `pom.xml`, `build.gradle`, `go.mod`, `Cargo.toml`, `requirements.txt`, `pyproject.toml`, etc.
- CLAUDE.md presence

### Step 3: Load marketplace catalog

Read `~/.claude/plugins/marketplaces/claude-plugins-official/.claude-plugin/marketplace.json` to get the full plugin catalog.

If the file doesn't exist, search under `~/.claude/plugins/marketplaces/` for any `marketplace.json`.
If still not found, tell the user: "Marketplace data not found. Please open the Discover tab with `/plugin` first."

### Step 4: Generate recommendations

Combine environment analysis + user needs + marketplace.json plugin info to make recommendations.

**Recommendation principles:**
- Exclude already installed plugins
- If a language is installed but its LSP plugin is not, recommend it
- If CLAUDE.md doesn't exist, recommend `claude-md-management`
- If no hooks are configured, recommend `hookify`
- For beginners with few plugins, prioritize general-purpose ones first
- Exclude OS-incompatible plugins (e.g. swift-lsp on Linux)
- Prioritize plugins matching the user's mentioned work areas
- Use marketplace.json descriptions to flexibly judge what's useful for the user's environment

**Priority order:**
1. Environment gaps: directly detected missing tools (e.g. Java installed but no jdtls-lsp)
2. User needs match: directly addresses mentioned work areas or frustrations
3. Foundation tools: CLAUDE.md, hooks, and other Claude Code base configuration
4. General productivity: workflow plugins not tied to a specific environment

**If no plugins to recommend:**
Give a positive message like "Your environment is already well-configured!"

### Step 5: Output results

Use this format (adapt language to match the user):

```
🔍 Environment Analysis
━━━━━━━━━━━━━━━━━━━━━━
OS: {os} {arch} | Languages: {languages}
Project: {project_type} | Installed plugins: {count}

💡 These plugins would make a difference
━━━━━━━━━━━━━━━━━━━━━━
1. {plugin_name} ({role description})
   {Clear 2-line explanation. Avoid jargon but be specific about what it does.}
   {Why it's needed given the current environment.}

2. ...

👉 Tell me which ones to install! (e.g. "install 1 and 3")
```

### Step 6: Install support

When the user selects numbers, install via Bash:
```bash
claude plugin install {plugin-name}@claude-plugins-official --scope user
```

Report success/failure, and on failure provide manual installation method:
"To install manually: `/plugin install {plugin-name}@claude-plugins-official`"
