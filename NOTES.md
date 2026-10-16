# NOTES.md - Claude Setup

## What was created

### 1. Created `CLAUDE.md` with project instructions for Claude Code:
- Project overview and tech stack
- Common commands (dev, build, test, lint)
- Code style and architecture rules
- Workflow and safety rules

### 2. Created `.claude/settings.json` with permissions:
- `allow`: safe read-only commands and `npm test` / `npm run lint`
- `ask`: potentially dangerous commands like `git push`, which require confirmation
- `deny`: blocked commands like reading `.env`, `git push --force`, and `rm -rf`

## Conventions - Use X, not Y format

Claude must follow these conventions without asking questions:

- Use `Validate input in route handlers, don't skip validation`
- Use `Access data through db/store.js, don't read files directly in routes`
- Use `Run npm test and npm run lint before committing, don't commit without testing`
- Use `Use async/await for async code, don't use nested callbacks`
- Use `Use named exports for utilities, don't use default exports`

## What was deliberately left out of CLAUDE.md and why

I deliberately left out the following to keep `CLAUDE.md` short, stable, and useful:

- **One-off notes:** Temporary TODOs, debugging notes, and specific bug fixes from one session. They become outdated fast and would confuse Claude in future sessions.
- **Obvious code details:** Things Claude can see directly in the code, like file names, function signatures, or obvious variable types. Duplicating them in `CLAUDE.md` creates maintenance overhead and risks getting out of sync.
- **Personal preferences / chat history:** Conversations about why a decision was made, or personal workflow preferences that are not project rules.
- **Secrets and environment values:** No API keys, passwords, or `.env` content. Those belong in environment variables, not in instructions.

`CLAUDE.md` only contains durable project rules that Claude needs in every session.

## Security - What could go wrong without the deny rules

The `deny` rules exist to prevent irreversible damage:

- **Reading `.env`:** Without this deny rule, Claude could read `.env` and expose secrets like API keys, database passwords, and tokens. If leaked, an attacker could access the production database or impersonate the app.
- **`git push --force`:** Without this deny rule, Claude could force-push and overwrite team history. This would destroy other developers' commits and break the shared `main` branch, requiring manual recovery.
- **`rm -rf`:** Without this deny rule, Claude could delete files or entire directories. This could wipe source code, configuration, or user data with no undo.

`ask` is used for `git push` because pushing is reversible in most cases but affects the team, so it needs human confirmation. `deny` is used for actions that are destructive or leak secrets.

## How to verify

I tested the setup in Claude Code with these steps and results:

1.  **Open the project in Claude Code.**
    Result: Claude loaded `CLAUDE.md` automatically on startup.

2.  **Ask Claude to explain the project - it should use `CLAUDE.md`.**
    Result: Claude explained the stack and commands using `CLAUDE.md` without asking for clarification.

3.  **Run `/memory`:**
    Result: `/memory` showed that `CLAUDE.md` was loaded as project memory. It listed the project instructions from the file, confirming Claude is using them.

4.  **Run `/permissions`:**
    Result: `/permissions` showed the rules from `.claude/settings.json`:
    - `allow` list includes `npm test` and `npm run lint`
    - `ask` list includes `git push`
    - `deny` list includes reading `.env`, `git push --force`, and `rm -rf`
    This confirms the permission file is active.

5.  **Run `npm test` and `npm run lint` - they should be allowed.**
    Result: Both commands ran without a permission prompt.

6.  **Try `git push` - Claude should ask for confirmation.**
    Result: Claude paused and asked for confirmation before pushing, as expected for `ask`.

7.  **Try to read `.env` - it should be denied.**
    Result: Claude refused to read `.env` and showed it was blocked by the `deny` rule.
