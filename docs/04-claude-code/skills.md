# Skills

A skill is a reusable workflow you define once and invoke with a slash command. Unlike a custom command (just a prompt file), a skill can include supporting scripts, context files and richer config.

---

## Skills vs CLAUDE.md vs subagents

| | CLAUDE.md | Skill | Subagent |
|---|---|---|---|
| When loaded | Every session | Only when invoked | When spawned |
| Purpose | Standing rules and context | Repeatable named workflow | Isolated task delegation |
| Lives in | Project root or ~/.claude/ | `.claude/skills/<name>/` | `.claude/agents/<name>.md` |
| Invoked by | Automatically | `/skill-name` or by Claude | Claude deciding to delegate |

Use a skill for multi-step workflows you run repeatedly: write a conventional commit message, review a PR, generate a changelog.

---

## Where skills live

```
.claude/
└── skills/
    └── conventional-commit/
        ├── SKILL.md        <- required
        └── template.md     <- optional supporting files
```

User-level skills (available in all projects) go in `~/.claude/skills/`.

---

## SKILL.md anatomy

```markdown
---
name: conventional-commit
description: Write a conventional commit message for the staged changes
---

Look at `git diff --staged` to understand what changed.
Write a conventional commit message following this format:

  <type>(<scope>): <short description>

  [optional body: why, not what]

Types: feat, fix, docs, style, refactor, perf, test, chore
Keep the first line under 72 characters.
Print the commit message and ask me to confirm before running `git commit`.
```

The `description` field matters a lot. Claude reads it to decide whether to auto-invoke the skill. Write it as a trigger: "when the user asks me to write a commit message, use this skill."

---

## Invoking a skill

Type the skill name as a slash command:
```
> /conventional-commit
```

Or just ask Claude. If the skill's description matches your request, Claude may invoke it automatically.

---

For building skills from scratch and sharing them with your team, see [Skills deep dive](../06-skills-deep-dive/index.md).

**Gotchas**

- After adding a new skill, Claude Code discovers it immediately. No restart needed.
- The `name` in the frontmatter becomes the slash command. Lowercase, hyphens only.
- Skills load into the current session when invoked. They don't sit in context by default.

---

> Sources: [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills) (fetched 2026-06-17)

Next: [Subagents](subagents.md) | See also: [Skills deep dive](../06-skills-deep-dive/index.md)
