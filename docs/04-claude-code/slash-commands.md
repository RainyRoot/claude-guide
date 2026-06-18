# Slash commands

Shortcuts you type in the Claude Code prompt to trigger specific behaviors. Some are built in, others you create yourself.

---

## Built-in commands

| Command | What it does |
|---|---|
| `/init` | Analyze the project and generate a starter CLAUDE.md |
| `/memory` | View and edit all CLAUDE.md, rules and auto memory files in this session |
| `/compact` | Summarize the conversation to free context space. Claude continues from the summary. |
| `/clear` | Start a fresh context (wipes the conversation, keeps settings) |
| `/mcp` | List connected MCP servers and their available tools |
| `/agents` | List available subagents defined in `.claude/agents/` |
| `/schedule` | Create a Routine (scheduled recurring Claude Code task) |
| `/desktop` | Hand off a terminal session to the Desktop app for visual diff review |
| `/context` | Show current context usage in tokens |

---

## Custom commands

Create your own slash commands. They live in `.claude/commands/` as markdown files. When you type `/filename`, Claude runs those instructions as a prompt.

Example:
```
.claude/
└── commands/
    └── review-pr.md
```

```markdown
Look at the last commit and the diff vs main.
Report:
1. Any logic bugs or edge cases not handled
2. Naming or style issues that violate our conventions (see CLAUDE.md)
3. Missing tests for new code paths

Be concise. List findings as bullet points. If everything looks fine, say so.
```

Now `/review-pr` works in any session.

User-level commands (available in all projects) go in `~/.claude/commands/`.

Commands can accept arguments via `$ARGUMENTS`:

```markdown
Search the codebase for "$ARGUMENTS" and explain what each result does in context.
```

Usage: `/search-and-explain getUserById`

---

## Tips

- Commands are just prompts, write them the same way you'd write a good chat message.
- Keep commands focused on a single task. Too many things in one command = unreliable results.
- Commands don't persist context between invocations. Each `/command` is a fresh prompt in the current session.

**Gotchas**

- Command files must have a `.md` extension.
- If two commands have the same name (one in the project, one in `~/.claude/commands/`), the project one wins.
- Changes to command files are picked up immediately, no restart needed.

---

> Sources: [code.claude.com/docs/en/commands](https://code.claude.com/docs/en/commands) (fetched 2026-06-17)

Next: [Skills](skills.md) | See also: [Skills deep dive](../06-skills-deep-dive/index.md)
