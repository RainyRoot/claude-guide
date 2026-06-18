# Tips and tricks

Practical patterns that make Claude Code more effective, cheaper and less frustrating.

---

## Context management

Claude has a context window. When it fills up, older parts of the conversation drop off. Claude doesn't warn you when this happens, it just starts forgetting earlier decisions.

Signs you're hitting limits:
- Claude contradicts something you agreed on earlier
- It "forgets" a file it already read
- Responses get shorter and more generic

Tools to manage context:

```
/compact    summarize the conversation. Claude continues from the summary.
            good for: mid-task when things are getting long

/clear      start fresh (wipes conversation, keeps settings and CLAUDE.md)
            good for: switching to a completely different task

/context    show how many tokens you've used vs the window size
            good for: knowing when to compact before you have to
```

Rule of thumb: `/compact` when you've been in a session for more than 30-40 minutes of active coding. Don't wait for context rot.

---

## Prompting patterns that work

**Be specific about what you want.**
Bad: "fix the bug."
Good: "The login endpoint returns 500 when the email is not in the database. It should return 401 with `{error: 'invalid credentials'}`. Fix this without changing the success path."

**Give examples.**
"Follow this pattern: [paste an existing function you're happy with]."

**Ask for a plan first.**
Before a big change: "Plan how you would refactor the auth module to use async/await throughout. Don't edit anything yet."
Review the plan, catch wrong assumptions, then say "proceed."

**Request a specific format.**
"Give me the result as a markdown table" or "output only the changed function, not the whole file."

**Iterate, don't restart.**
If Claude's answer is 80% right, say what's wrong: "Good start. The error handling is right but the response format should be `{error: '...'}` not a string." Don't start a new session.

**Tell Claude what NOT to do.**
"Refactor this without changing the public API surface" or "do not touch the database migration files."

---

## Common beginner mistakes

**One giant prompt.** Asking Claude to "build me a full stack todo app" in one message usually produces generic code that doesn't fit your project. Break it down. Data model first, then API, then frontend.

**No CLAUDE.md.** Claude starts every session knowing nothing about your project. Without a CLAUDE.md you re-explain conventions every time.

**Never reviewing diffs.** Claude is good but not perfect. Read every diff before approving. A bad edit you approve is harder to clean up than one you catch in the moment.

**YOLO mode on a real repo.** Running `--dangerously-skip-permissions` in a project with real credentials or files you can't lose is how accidents happen. See [Permissions and safety](../09-permissions-and-safety/index.md).

**Trusting Claude on current info.** Claude's knowledge has a cutoff date. For anything recent like library versions, API changes or current prices, ask Claude to use web search or verify yourself.

---

## Cost control

If you're on the API (not a flat subscription) these habits keep costs down:

**Pick the right model.** Haiku 4.5 costs about 1/10th of Fable 5. For simple high-volume tasks it's usually good enough. See [Choosing a model](../02-models/index.md).

**Use subagents for noisy reads.** Analyzing 50 files in a subagent keeps that cost out of your main session. See [Subagents](../04-claude-code/subagents.md).

**Scope tasks tightly.** "Check `src/auth/login.ts` for XSS vulnerabilities" is cheaper than "check the whole codebase for security issues."

**Compact before long sessions.** A compacted context is much cheaper than a full 2-hour transcript sitting in context.

---

## 10 things worth knowing

1. `/init` generates a useful CLAUDE.md from your codebase. Run it on every new project.
2. `Shift+Tab` cycles through modes (normal, auto-accept, plan). Use plan mode before big changes.
3. Claude can run git commands. "Commit this with a conventional message" works.
4. You can pipe into Claude: `tail -100 server.log | claude -p "any errors?"`.
5. Subagents keep your main context clean. Use them for research and review tasks.
6. CLAUDE.md under 200 lines is followed more reliably than a 500 line wall of text.
7. Ask Claude to explain what it's about to do before it does it. Catches misunderstandings early.
8. You can `/compact` mid-task and continue. It doesn't lose track of what you were building.
9. Hooks enforce things that must happen (formatting, lint). CLAUDE.md only asks nicely.
10. Small commits with conventional messages make `git log` useful and `git bisect` possible.

**Gotchas**

- Compacting doesn't save tool outputs from earlier in the session. If Claude read a file before `/compact`, it may need to read it again.
- Claude doesn't retain memory between sessions by default. Auto memory and CLAUDE.md bridge this.

---

> Sources: [code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices) (fetched 2026-06-17)

Next: [Permissions and safety](../09-permissions-and-safety/index.md) | See also: [Project workflows](../07-project-workflows/index.md), [Subagents](../04-claude-code/subagents.md)
