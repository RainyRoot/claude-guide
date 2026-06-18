# Permissions and safety

## How do I turn on unrestricted mode?

Short answer: you can't, because it doesn't exist.

Claude has the same values in every interface. No toggle, no secret flag, no jailbreak removes Claude's judgment. When people ask about "unrestricted mode" they usually mean one of two things:

1. They want Claude to stop asking for approval on every file edit and command.
2. They've heard of YOLO mode and want to know if it's real.

Both are real things but neither is what the name suggests.

---

## What people actually mean: permission modes

Claude Code has three levels of approval prompting. None of them change Claude's values. They just control how often Claude stops to ask before acting.

### Normal mode (default)

Claude asks for your approval before every file edit and every shell command. You see each proposed change before it happens.

```
Claude wants to edit src/login.ts:
  - Line 42: return null  ->  return { error: "unauthorized" }
[y/n]?
```

Right default for most work. You keep full control.

### Auto-accept edits

Claude edits files without asking but still asks before running shell commands. Activate with `Shift+Tab`.

Good for: when you trust Claude's edits and want to move fast, but still want to review before any git operations run.

### Bypass permissions (--dangerously-skip-permissions)

```bash
claude --dangerously-skip-permissions
```

This is the YOLO mode. Claude skips all approval prompts and will edit files, run shell commands, commit to git and execute anything it decides is necessary without asking.

---

## What bypass mode actually removes

It removes the approval prompts.

It does NOT remove: Claude's judgment, its attempt to be careful, its knowledge of what's risky.

Claude still tries to do the right thing. The difference is that if it makes a mistake, it will have already made it by the time you notice.

---

## When it's safe to use bypass mode

- Disposable repo you can delete or hard reset with zero regret
- No real credentials anywhere in scope (no .env files with API keys, no production DB strings)
- No production access from this machine
- Git is clean before you start so you have a fallback
- Ideally in a container or VM, not your main machine

If you can check all of that, bypass mode is fine for fast iteration.

---

## When NOT to use it

- Your `.env` has real credentials. Claude might read it, use it or accidentally commit it.
- You have production database access. An agent running `DROP TABLE users` without asking is not hypothetical.
- Your project uses file deletion commands. Claude will run them.
- You're on a shared machine or a CI runner with broad permissions.
- It's your first session in an unfamiliar repo.

---

## A note on jailbreaks

Jailbreaks (prompts or techniques that try to trick Claude into ignoring its training) exist, evolve and mostly don't work reliably, especially not on Claude Code which runs on up-to-date models. Even when a jailbreak appears to work the behavior is inconsistent.

If Claude is declining something you need, the better path is:
- Rephrase the request to be more specific about your legitimate use case
- Check if there's a different approach that accomplishes the same goal

There's no secret setting.

---

## Hooks for actual enforcement

If something must always happen regardless of Claude's decisions, use [hooks](../04-claude-code/hooks.md) not approval prompts.

Hooks run as shell commands and cannot be bypassed by Claude. Right tool for hard requirements: "always run lint before a commit," "never allow rm -rf," "log every file edit."

---

> Sources: [code.claude.com/docs/en/settings](https://code.claude.com/docs/en/settings) (fetched 2026-06-17)

Next: [Cookbook](../10-cookbook/index.md) | See also: [Plan mode and permissions](../04-claude-code/plan-and-permissions.md), [Hooks](../04-claude-code/hooks.md)
