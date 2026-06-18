# CLAUDE.md

Every Claude Code session starts with an empty context window. CLAUDE.md is how you give Claude standing instructions that persist across sessions. Project conventions, build commands, naming rules, things Claude should always or never do.

---

## Two memory systems

| | CLAUDE.md | Auto memory |
|---|---|---|
| Who writes it | You | Claude, automatically |
| What it contains | Instructions and rules | Learnings and patterns it discovers |
| Scope | Project, user or org | Per repository |
| Loaded | Every session | Every session (first 200 lines of MEMORY.md) |
| Use for | Coding standards, architecture, workflows | Build commands, debugging insights Claude picks up |

Use CLAUDE.md for explicit reliable rules. Auto memory handles the "Claude figured it out last session" cases on its own.

---

## Where to put it

| Scope | Location | Use for | Shared with |
|---|---|---|---|
| Organization | `/etc/claude-code/CLAUDE.md` (Linux) | Company-wide policies | Everyone on the machine |
| User personal | `~/.claude/CLAUDE.md` | Personal preferences across all projects | Just you |
| Project shared | `./CLAUDE.md` or `./.claude/CLAUDE.md` | Team standards, project conventions | Your team via git |
| Local personal | `./CLAUDE.local.md` | Your sandbox URLs, personal test data | Just you (add to .gitignore) |

Files in parent directories are also loaded. If you run Claude in `myrepo/backend/`, it reads `myrepo/CLAUDE.md`, `myrepo/backend/CLAUDE.md` and any `CLAUDE.local.md` files along the way.

---

## Set up your first CLAUDE.md

Run `/init` in Claude Code and it will analyze your project and generate a starter file:

```bash
cd my-project
claude
> /init
```

Claude explores your codebase, finds build commands, test commands and conventions, and writes a draft. You refine from there.

A solid project CLAUDE.md looks something like this:

```markdown
# Project: my-api

## Build and test
- Run: `npm run dev`
- Test: `npm test`
- Lint: `npm run lint` (run before every commit)

## Conventions
- All handlers live in `src/handlers/`
- Use async/await, not raw .then() chains
- Error responses: { error: "message", code: "ERROR_CODE" }
- Never commit directly to main, always branch

## Architecture
- Express + TypeScript
- Auth via JWT, tokens expire in 24h
- DB: Postgres via Prisma ORM
```

---

## Importing other files

CLAUDE.md can pull in other files with `@path/to/file` syntax:

```markdown
See @README.md for project overview.
@docs/architecture.md
```

Imported files are loaded at session start just like the CLAUDE.md itself.

---

## Path-scoped rules (.claude/rules/)

For larger projects you can scope instructions to specific file types or directories. Rules without a `paths` field load always. Rules with `paths` only load when Claude is working with matching files.

```
.claude/
└── rules/
    ├── testing.md          <- loads always
    └── frontend.md         <- only loads for matching files
```

```markdown
---
paths:
  - "src/components/**/*.tsx"
---

# React components
- Use functional components, never class components
- Prefer named exports
```

---

## Auto memory

Claude writes notes to itself in `~/.claude/projects/<repo>/memory/`. You don't manage this, Claude does it when it learns something worth remembering (a build command, a pattern you corrected, a workflow).

To see what it saved: type `/memory` in a session. The files are plain markdown you can read, edit or delete.

To disable auto memory:
```json
// .claude/settings.json
{
  "autoMemoryEnabled": false
}
```

---

## What belongs where

| Belongs in CLAUDE.md | Belongs elsewhere |
|---|---|
| Rules true for every session | Multi-step procedures -> Skills |
| Build/test/lint commands | One-off task instructions -> type them in chat |
| Naming conventions | Secrets, credentials -> environment variables |
| Architecture decisions | Personal preferences -> `CLAUDE.local.md` |

Keep it under 200 lines. Longer files consume more context and Claude follows them less reliably.

**Gotchas**

- CLAUDE.md is loaded as context, not enforced config. Claude reads it and tries to follow it but can deviate on vague or conflicting instructions. For hard enforcement use [hooks](hooks.md).
- After editing a CLAUDE.md you don't need to restart. The file is read fresh at the start of each new session.
- If Claude isn't following it, run `/memory` to verify it's being loaded. Common issue: the file is in the wrong location.

---

> Sources: [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory) (fetched 2026-06-17)

Next: [Slash commands](slash-commands.md) | See also: [Project workflows](../07-project-workflows/index.md)
