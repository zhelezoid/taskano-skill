# Taskano skill

The Claude skill for [Taskano](https://taskano.app): AI agents manage the work, people do it in Telegram,
each in their own language.

**Start with [PHILOSOPHY.md](PHILOSOPHY.md)** — what the product believes, what it needs to know about your
people to work at all, and which capability switches on at which point as a company grows.

Taskano is an invite-only pilot. Either your company has already been set up for you — then you just sign in with
your email — or you need an invite code from whoever invited you.

## What you need
- A claude.ai account with this skill loaded — that is where you run the company from.
- A Telegram account for every person who will receive tasks.
- A Taskano invite code — unless your company has already been set up for you.

Optional, and only once people start waiting for answers while you are away: a cloud routine, so the agents keep
working when no session of yours is open. It needs Claude Code on the web and a GitHub account (the routine reads
this skill from a repository). Skip it at the start — setup.md explains when it is worth connecting.

## Setup
1. **Connector.** In claude.ai: **Settings → Connectors → Add custom connector**. Name it `Taskano`, URL
   `https://taskano.app/mcp`. Open it and sign in with the code sent to your email.
2. **Skill.** In claude.ai: **Settings → Capabilities → Skills → Upload skill**, and upload the `skills/taskano`
   folder from this repository as a ZIP archive. (If you also use the Claude Code CLI, copy the same folder to
   `~/.claude/skills/taskano` there.)
3. Tell Claude: **"Set up my company in Taskano"**. Claude takes it from there in conversation: people, areas,
   and the first task. From then on the agents do their work inside your own sessions — every time you open a chat,
   they catch up on what has piled up.
4. Later, if people start waiting while you are away: fork this repository and set up the cloud routine
   (setup.md → "Agent runner").

## Contents
- `PHILOSOPHY.md` — the idea, what the system needs from you, how it grows.
- `skills/taskano/SKILL.md` — entry point.
- `skills/taskano/setup.md` — setting up an organization.
- `skills/taskano/assigning.md` — the craft of handing work over: when to record, one task or a plan, who does it.
- `skills/taskano/leading.md` — how agents lead people (the routine runner reads it on every run).
- `skills/taskano/review.md` — reviewing your own tasks one at a time.
- `routine-prompt.md` — the prompt for the cloud routine.

Minimum Taskano server version: 0.1.0 (`get_capabilities`).
