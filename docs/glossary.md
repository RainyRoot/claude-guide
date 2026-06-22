# Glossary

Key terms in alphabetical order. Each one links to where it's explained in detail.

---

**Adaptive thinking**
Claude decides on its own how deeply to think before answering, no configuration needed. Always on in Fable 5 and Opus 4.8. See [Choosing a model](02-models/index.md#extended-and-adaptive-thinking).

**Agent loop**
The cycle Claude Code runs through on every task: read your request, read files, run commands, propose edits, wait for approval, repeat. See [Claude Code](04-claude-code/index.md#the-agent-loop).

**Auto memory**
Notes Claude writes to itself across sessions, stored in `~/.claude/projects/<repo>/memory/`. Different from CLAUDE.md which you write yourself. See [CLAUDE.md](04-claude-code/claude-md.md#auto-memory).

**Bypass permissions**
Running Claude Code with `--dangerously-skip-permissions` so it never stops to ask for approval. Also called YOLO mode. Only safe on throwaway sandboxes with no real credentials. See [Permissions and safety](09-permissions-and-safety/index.md).

**CLAUDE.md**
A markdown file in your project that Claude Code reads at the start of every session. The right place for standing rules, conventions and build commands. See [CLAUDE.md](04-claude-code/claude-md.md).

**Claude Code**
Anthropic's agentic coding tool. Available as a terminal CLI, VS Code/Cursor/JetBrains extension, desktop app and web app. See [Claude Code](04-claude-code/index.md).

**Claude Cowork**
Anthropic's agentic knowledge work desktop app, aimed at non-developers. See [Overview](01-overview/index.md).

**Context window**
The maximum amount of text Claude can hold in one session, measured in tokens. Once it fills up, older content drops off. See [Tips and tricks](08-tips-and-tricks/index.md#context-management).

**Extended thinking**
A mode where Claude shows its reasoning step by step before answering. Slower but more accurate on hard problems. Available on Sonnet 4.6 and Haiku 4.5. See [Choosing a model](02-models/index.md#extended-and-adaptive-thinking).

**Fable 5**
Anthropic's most capable widely released model as of June 2026. Uses adaptive thinking and has a 1M token context window. See [Choosing a model](02-models/index.md).

**Haiku**
The fastest and cheapest Claude model tier. Good for simple high-volume tasks. See [Choosing a model](02-models/index.md).

**Hooks**
Shell commands that Claude Code runs automatically on lifecycle events, like formatting a file after every edit or blocking a dangerous command. See [Hooks](04-claude-code/hooks.md).

**MCP (Model Context Protocol)**
An open standard for connecting Claude to external tools and data sources like GitHub, Slack or Google Drive. See [MCP](04-claude-code/mcp.md).

**Opus**
Claude's most capable model tier after Fable. Opus 4.8 is the current latest. See [Choosing a model](02-models/index.md).

**Plan mode**
A Claude Code mode where Claude plans what it would do but does not edit any files until you approve. Activate with `Shift+Tab`. See [Plan mode and permissions](04-claude-code/plan-and-permissions.md).

**Project**
A workspace in claude.ai that groups related conversations and holds shared instructions and files. See [The chat app](03-chat-app/index.md#projects).

**Routines**
Scheduled Claude Code sessions that run on Anthropic's infrastructure even when your computer is off. Create them with `/schedule`. See [Tips and tricks](08-tips-and-tricks/index.md).

**Skill**
A reusable workflow stored in `.claude/skills/<name>/SKILL.md`. Invokable as a slash command like `/skill-name`. See [Skills](04-claude-code/skills.md) and [Skills deep dive](06-skills-deep-dive/index.md).

**Slash command**
A `/command` you type in Claude Code to trigger specific behavior. Built-ins include `/init`, `/compact`, `/clear`, `/memory`, `/mcp` and `/agents`. Custom commands live in `.claude/commands/`. See [Slash commands](04-claude-code/slash-commands.md).

**Sonnet**
Claude's balanced model tier, fast and smart. Sonnet 4.6 is the current latest and the default for most coding work. See [Choosing a model](02-models/index.md).

**Subagent**
A specialized AI that runs in its own isolated context window. Good for tasks that would flood your main session with noise, like reviewing 50 files or analyzing logs. See [Subagents](04-claude-code/subagents.md).

**Token**
The unit Claude uses to measure text. Roughly 1 token is about 3/4 of an English word. Context windows and API pricing are both measured in tokens. See [Choosing a model](02-models/index.md).

**YOLO mode**
See Bypass permissions.
