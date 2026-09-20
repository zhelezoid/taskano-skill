# Taskano skill

The Claude skill for [Taskano](https://taskano.app): AI agents manage the work, people do it in Telegram,
each in their own language.

Taskano is an invite-only pilot. Either your company has already been set up for you — then you just sign in with
your email — or you need an invite code from whoever invited you.

## What you need
- A claude.ai account with Claude Code on the web (the agent runner is a cloud routine).
- A GitHub account (the routine reads this skill from a repository, so you fork this one).
- A Telegram account for every person who will receive tasks.
- A Taskano invite code — unless your company has already been set up for you.

## Setup
1. **Connector.** In claude.ai: **Settings → Connectors → Add custom connector**. Name it `Taskano`, URL
   `https://taskano.app/mcp`. Open it and sign in with the code sent to your email.
2. **Skill.** In claude.ai: **Settings → Capabilities → Skills → Upload skill**, and upload the `skills/taskano`
   folder from this repository as a ZIP archive. (If you also use the Claude Code CLI, copy the same folder to
   `~/.claude/skills/taskano` there.)
3. **Fork this repository** into your own GitHub account. The routine you create during setup reads the skill
   from your fork.
4. Tell Claude: **"Set up my company in Taskano"**. Claude takes it from there in conversation: people, areas,
   the agent runner, and the first task.

## Contents
- `skills/taskano/SKILL.md` — entry point.
- `skills/taskano/setup.md` — setting up an organization.
- `skills/taskano/leading.md` — how agents lead people (the routine runner reads it on every run).
- `skills/taskano/review.md` — reviewing your own tasks one at a time.
- `routine-prompt.md` — the prompt for the cloud routine.

Minimum Taskano server version: 0.1.0 (`get_capabilities`).
