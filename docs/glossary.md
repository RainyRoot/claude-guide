# Glossary

Key terms in alphabetical order. Each one links to where it's explained in detail.

---

**Adaptive thinking** - a reasoning mode where Claude decides how deeply to think before answering. Always on in Fable 5 and Opus 4.8. See [Choosing a model](02-models/index.md#extended-and-adaptive-thinking).

**Agent loop** - the cycle Claude Code runs: read your request, read files, run commands, propose edits, wait for approval, repeat. See [Claude Code](04-claude-code/index.md#the-agent-loop).

**Auto memory** - notes Claude writes to itself across sessions, stored in `~/.claude/projects/<repo>/memory/`. Different from CLAUDE.md which you write yourself. See [CLAUDE.md](04-claude-code/claude-md.md#auto-memory).

**Bypass permissions / YOLO mode** - running Claude Code with `--dangerously-skip-permissions` so it never asks for approval. Only safe on throwaway sandboxes. See [Permissions and safety](09-permissions-and-safety/index.md).

**CLAUDE.md** - a markdown file in your project that Claude Code reads at the start of every session. For standing rules, conventions, build commands. See [CLAUDE.md](04-claude-code/claude-md.md).

**Claude Code** - Anthropic's agentic coding tool. Available as a terminal CLI, VS Code/Cursor/JetBrains extension, desktop app and web app. See [Claude Code](04-claude-code/index.md).

**Claude Cowork** - Anthropic's agentic knowledge work desktop app, aimed at non-developers. See [Overview](01-overview/index.md).

**Context window** - the maximum amount of text Claude can hold in one session, measured in tokens. Once it fills up, older content drops off. See [Tips and tricks](08-tips-and-tricks/index.md#context-management).

**Extended thinking** - a mode where Claude shows its reasoning step by step before answering. Available on Sonnet 4.6 and Haiku 4.5. See [Choosing a model](02-models/index.md#extended-and-adaptive-thinking).

**Fable 5** - Anthropic's most capable widely released model as of June 2026. Uses adaptive thinking, 1M token context. See [Choosing a model](02-models/index.md).

**Hooks** - shell commands that Claude Code runs automatically on lifecycle events. See [Hooks](04-claude-code/hooks.md).

**Haiku** - the fastest and cheapest Claude model tier. Good for simple high-volume tasks. See [Choosing a model](02-models/index.md).

**MCP (Model Context Protocol)** - an open standard for connecting Claude to external tools and data sources like GitHub, Slack, Google Drive. See [MCP](04-claude-code/mcp.md).

**Opus** - Claude's most capable model tier after Fable. Opus 4.8 is the current latest. See [Choosing a model](02-models/index.md).

**Plan mode** - a Claude Code mode where Claude plans what it would do but does not edit files until you approve. Activate with Shift+Tab. See [Plan mode and permissions](04-claude-code/plan-and-permissions.md).

**Project (chat app)** - a workspace in claude.ai that groups conversations and holds shared instructions and files. See [The chat app](03-chat-app/index.md#projects).

**Routines** - scheduled Claude Code sessions that run on Anthropic's infrastructure even when your computer is off. Create via `/schedule`. See [Tips and tricks](08-tips-and-tricks/index.md).

**Skill** - a reusable workflow stored in `.claude/skills/<name>/SKILL.md`. Invokable as a slash command. See [Skills](04-claude-code/skills.md) and [Skills deep dive](06-skills-deep-dive/index.md).

**Slash command** - a `/command` you type in Claude Code. Built-ins include `/init`, `/compact`, `/clear`, `/memory`, `/mcp`, `/agents`. Custom commands live in `.claude/commands/`. See [Slash commands](04-claude-code/slash-commands.md).

**Sonnet** - Claude's balanced model tier, fast and smart. Sonnet 4.6 is the current latest and the common default for coding. See [Choosing a model](02-models/index.md).

**Subagent** - a specialized AI that runs in its own isolated context window. Good for tasks that would flood your main session with noise. See [Subagents](04-claude-code/subagents.md).

**Token** - the unit Claude uses to measure text. Roughly 1 token is about 3/4 of an English word. Context windows and API pricing are measured in tokens. See [Choosing a model](02-models/index.md).

**YOLO mode** - see Bypass permissions.
