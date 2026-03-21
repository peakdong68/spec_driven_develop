**English** | [中文](./README.zh-CN.md)

# Spec-Driven Develop

A cross-platform AI agent skill that automates the pre-development workflow for large-scale complex tasks.

When you tell your AI agent something like "rewrite this project in Rust" or "migrate to a microservice architecture", the agent automatically executes a standardized preparation pipeline before any code is written:

1. Deep project analysis
2. Task decomposition and planning
3. Progress tracking documentation
4. Task-specific sub-SKILL generation
5. Iterative development with document-driven progress awareness

The entire workflow is document-driven: a master progress file serves as the agent's "memory anchor" across conversations, so it never loses track of where things stand.

## Supported Platforms

- **Claude Code** — installed as a plugin (with enhanced agent/command support)
- **Codex (OpenAI)** — installed as a skill
- **Cursor** — installed as a global or project-level skill

## Installation

### Claude Code

Add the marketplace and install the plugin:

```
/plugin marketplace add zhu1090093659/spec_driven_develop
/plugin install spec-driven-develop@spec-driven-develop
```

After installation, run `/reload-plugins` to activate.

### Codex CLI

Use the built-in skill installer (inside a Codex session):

```
$skill-installer install https://github.com/zhu1090093659/spec_driven_develop/tree/main/plugins/spec-driven-develop/skills/spec-driven-develop
```

Or install via shell:

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-codex.sh)
```

### Cursor

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-cursor.sh)
```

Or clone the repo and run locally:

```bash
git clone https://github.com/zhu1090093659/spec_driven_develop.git
bash spec_driven_develop/scripts/install-cursor.sh
```

## Usage

### Automatic Trigger

Simply describe your large-scale task to the agent. The skill triggers on keywords like:

- English: "rewrite", "migrate", "overhaul", "refactor entire project", "transform", "rebuild in [language]"
- Chinese: "改造", "重写", "迁移", "重构", "大规模"

### Manual Trigger (Claude Code)

```
/spec-dev rewrite this Python project in Rust
```

### Cross-Conversation Continuity

When working on a long-running task across multiple conversations, the agent reads `docs/progress/MASTER.md` at the start of each new conversation to restore context and continue from where it left off.

### Cleanup

When all tasks are marked complete in the master progress file, the agent enters cleanup mode: it asks which artifacts you want to keep and removes the rest.

## Project Structure

```
spec_driven_develop/
├── .claude-plugin/
│   └── marketplace.json                   # Claude Code marketplace catalog
├── plugins/spec-driven-develop/           # Self-contained Claude Code plugin
│   ├── .claude-plugin/plugin.json         # Plugin manifest
│   ├── skills/spec-driven-develop/
│   │   ├── SKILL.md                       # Core skill (works on all platforms)
│   │   └── references/doc-templates.md    # Document templates
│   ├── agents/                            # Claude Code sub-agents
│   │   ├── project-analyzer.md
│   │   └── task-architect.md
│   └── commands/spec-dev.md               # /spec-dev slash command
├── scripts/                               # Installation scripts
│   ├── install-cursor.sh
│   ├── install-codex.sh
│   └── install-all.sh
└── LICENSE
```

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=zhu1090093659/spec_driven_develop&type=Date)](https://star-history.com/#zhu1090093659/spec_driven_develop&Date)

## License

MIT
