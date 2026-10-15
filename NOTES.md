# Notes - Set up Claude Code

## What was done

Created `CLAUDE.md` with project instructions for Claude Code:
- Project overview and tech stack
- Common commands (dev, build, test, lint)
- Code style and architecture rules
- Workflow and safety rules

Created `.claude/settings.json` with permissions:
- `allow`: safe read-only commands and `npm test` / `npm run lint`
- `ask`: potentially dangerous commands like `git push`, which require confirmation
- `deny`: blocked commands like reading `.env`, `git push --force`, and `rm -rf`

## Why this setup

- `CLAUDE.md` gives Claude consistent context in every session, so it follows the same rules.
- `settings.json` separates safe, confirmable, and forbidden actions, reducing the risk of accidental destructive commands.

## How to verify

1. Open the project in Claude Code.
2. Ask Claude to explain the project - it should use `CLAUDE.md`.
3. Run `npm test` and `npm run lint` - they should be allowed.
4. Try `git push` - Claude should ask for confirmation.
5. Try to read `.env` - it should be denied.
