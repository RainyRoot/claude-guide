# Hooks

Shell commands that Claude Code runs automatically at specific points in its lifecycle. Before or after file edits, before running a command, after a session ends.

---

## When to use hooks vs CLAUDE.md

- CLAUDE.md: "Claude, please run `npm run lint` before committing." Claude tries to follow this but can forget.
- Hook: run `npm run lint` unconditionally after every file edit. Claude has no say.

If it must happen, use a hook. If it's guidance, use CLAUDE.md.

---

## Hook events

| Event | When it fires |
|---|---|
| `PreToolUse` | Before Claude calls any tool (edit, bash, read...) |
| `PostToolUse` | After Claude calls a tool |
| `PreCompact` | Before Claude compacts the conversation |
| `Stop` | When the session ends or Claude finishes a task |
| `InstructionsLoaded` | When CLAUDE.md files finish loading (useful for debugging) |

---

## Configuring hooks

Hooks go in `.claude/settings.json` (project level) or `~/.claude/settings.json` (user level):

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npm run lint --silent"
          }
        ]
      }
    ]
  }
}
```

The `matcher` is a regex matched against the tool name. `Edit|Write` fires after any file edit.

---

## Example 1: auto-format after every edit

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write \"$CLAUDE_TOOL_INPUT_FILE_PATH\""
          }
        ]
      }
    ]
  }
}
```

`$CLAUDE_TOOL_INPUT_FILE_PATH` is set by Claude Code to the path of the file being edited.

---

## Example 2: block dangerous commands

Prevent `rm -rf` from running:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$CLAUDE_TOOL_INPUT_COMMAND\" | grep -q 'rm -rf' && echo 'BLOCKED: rm -rf requires manual approval' && exit 1 || exit 0"
          }
        ]
      }
    ]
  }
}
```

A hook that exits non-zero blocks the tool call.

---

## Example 3: log every file Claude edits

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$(date -u +%Y-%m-%dT%H:%M:%SZ) EDITED: $CLAUDE_TOOL_INPUT_FILE_PATH\" >> ~/.claude/edit-log.txt"
          }
        ]
      }
    ]
  }
}
```

---

## Environment variables in hooks

| Variable | Value |
|---|---|
| `$CLAUDE_TOOL_NAME` | Name of the tool being called (Edit, Bash...) |
| `$CLAUDE_TOOL_INPUT_FILE_PATH` | File path for Edit/Write/Read tools |
| `$CLAUDE_TOOL_INPUT_COMMAND` | Shell command for the Bash tool |

**Gotchas**

- Hooks run with the same permissions as your user. Don't put secrets in hook commands.
- A hook that exits non-zero blocks the tool call and shows the output to Claude. Claude sees the error and may adjust.
- Changes to settings.json take effect immediately.

---

> Sources: [code.claude.com/docs/en/hooks-guide](https://code.claude.com/docs/en/hooks-guide) (fetched 2026-06-17)

Next: [MCP](mcp.md) | See also: [Permissions and safety](../09-permissions-and-safety/index.md)
