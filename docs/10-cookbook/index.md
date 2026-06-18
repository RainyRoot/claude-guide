# Cookbook

Step by step recipes. Each one: goal, exact steps, expected result, what to watch for.

---

## Recipe A: Review your last commit with a code review subagent

**Goal:** catch bugs and style issues in the most recent commit without flooding your main session.

**Setup (one time):**

```bash
mkdir -p .claude/agents
```

Create `.claude/agents/code-reviewer.md`:

```markdown
---
name: code-reviewer
description: Review code changes for bugs, style issues and missing tests. Use when the user asks to review code, check a commit, audit changes or do a PR review.
tools:
  - Read
  - Bash
model: claude-sonnet-4-6
---

You are a careful code reviewer. Catch real bugs, not style nitpicks unless they conflict with CLAUDE.md conventions.

Run `git diff HEAD~1 HEAD` to see the most recent commit.

For each changed file report:
1. Bugs - logic errors, unhandled edge cases, null or undefined risks
2. Convention violations - conflicts with rules in CLAUDE.md
3. Missing tests - new logic paths with no test coverage
4. Security - obvious injection risks, credential exposure, access control gaps

Format as markdown bullets. Be concise. End with: LGTM / MINOR ISSUES / BLOCKING ISSUES.
```

**Steps:**

1. Make your changes and commit: `git commit -m "feat: add login endpoint"`
2. Start Claude Code: `claude`
3. Ask for a review:
   ```
   > Use the code-reviewer subagent to review my last commit.
   ```
4. Claude spawns the subagent which reads the diff and reports findings. Your main context stays clean.

**What to watch for:** if the subagent finds blocking issues, ask Claude to fix them before pushing. Add your convention rules to CLAUDE.md so reviews are project-aware.

---

## Recipe B: Build your first skill

**Goal:** a `/standup` skill that generates a daily standup update from recent git commits.

**Steps:**

1. Create the skill directory:
   ```bash
   mkdir -p .claude/skills/standup
   ```

2. Create `.claude/skills/standup/SKILL.md`:
   ```markdown
   ---
   name: standup
   description: Generate a daily standup update based on git commits from the last 24 hours. Use when the user asks for a standup, what they worked on or a summary of recent work.
   ---

   Run: `git log --oneline --since="24 hours ago" --author="$(git config user.name)"`

   Summarize the commits into a standup format:

   **Yesterday:**
   [bullet points of completed work in plain English, not commit messages]

   **Today:**
   [ask me what I'm planning, or infer from obvious in-progress work]

   **Blockers:**
   [none, or ask me if there are any]

   Keep it short enough to paste into Slack.
   ```

3. Use it:
   ```
   > /standup
   ```

**What to watch for:** the skill reads the git author name from your config. If you use different names across machines it may miss some commits.

---

## Recipe C: Vibe-code a small app from zero

**Goal:** build a simple REST API with Claude doing most of the coding. You review, approve and commit.

**Steps:**

1. Set up the project:
   ```bash
   mkdir todo-api && cd todo-api
   git init
   npm init -y
   claude
   ```

2. Create a PLAN.md:
   ```
   > Create a PLAN.md for a simple todo list REST API. Stack: Node.js + Express + TypeScript, in-memory storage (no database yet), endpoints: GET /todos, POST /todos, DELETE /todos/:id. Include the file structure.
   ```
   Review and approve what Claude writes.

3. Create a CLAUDE.md:
   ```
   > Run /init to generate a CLAUDE.md. Add: always use async/await, error format is { error: string }.
   ```

4. Build in milestones:
   ```
   > Build the basic Express app skeleton with TypeScript config. No routes yet.
   ```
   Review the diff. Approve. Commit:
   ```
   > Commit this with a conventional message.
   ```
   Then continue:
   ```
   > Add the GET /todos endpoint. Write a test for it.
   ```
   Review, approve, commit. Repeat for each endpoint.

5. Update PROGRESS.md before each commit:
   ```
   > Update PROGRESS.md: mark "Express skeleton" as done, set "GET /todos" as in progress.
   ```

**What to watch for:** review every diff. Claude makes mistakes, especially on TypeScript types and edge cases. If it goes in the wrong direction, say "stop, that's not what I meant" and correct it immediately.

---

## Recipe D: Set up a repo so future Claude sessions are productive immediately

**Goal:** configure a new or existing repo so any future Claude Code session starts knowing the project.

**Steps:**

1. Generate a CLAUDE.md:
   ```bash
   cd my-project
   claude
   > /init
   ```
   Review the generated file, add anything it missed.

2. Create a project PLAN.md:
   ```
   > Write a PLAN.md that captures the current state of this project: what it is, the file structure, the main features that exist and what we're working on next.
   ```

3. Create a PROGRESS.md:
   ```
   > Create a PROGRESS.md with a status table. Current status: initial setup done. List the main planned milestones as todo.
   ```

4. Add a code-review subagent (copy from Recipe A).

5. Commit everything:
   ```bash
   git add CLAUDE.md PLAN.md PROGRESS.md .claude/
   git commit -m "chore: add Claude Code project configuration"
   ```

**What to watch for:** next time you or a teammate runs `claude` in this project, it immediately understands the context, conventions and current state.

---

## Recipe E: Connect a GitHub MCP server

**Goal:** let Claude read your GitHub issues and PRs directly.

**Steps:**

1. Get a GitHub personal access token with `repo` scope at [github.com/settings/tokens](https://github.com/settings/tokens).

2. Add the MCP server to `~/.claude/settings.json`:
   ```json
   {
     "mcpServers": {
       "github": {
         "type": "stdio",
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-github"],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token_here"
         }
       }
     }
   }
   ```

   Don't commit a file with your token in it. Use the user-level settings file (`~/.claude/settings.json`) not the project one.

3. Restart Claude Code and run `/mcp` to verify the server appears.

4. Use it:
   ```
   > List the open issues in this repo and summarize any that look like quick wins.
   ```

**What to watch for:** the token needs the right scopes. `repo` for private repos, `public_repo` is enough for public only. Claude's GitHub MCP calls count against your GitHub API rate limit.

---

> Sources: [code.claude.com/docs/en](https://code.claude.com/docs/en) (fetched 2026-06-17)

See also: [Subagents](../04-claude-code/subagents.md), [Skills deep dive](../06-skills-deep-dive/index.md), [Project workflows](../07-project-workflows/index.md), [MCP](../04-claude-code/mcp.md)
