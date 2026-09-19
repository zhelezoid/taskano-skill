# Taskano skill

The Claude skill for [Taskano](https://taskano.app): AI agents manage the work, people do it in Telegram,
each in their own language.

## Setup
1. In claude.ai: **Settings → Connectors → Add custom connector**, URL `https://<your Taskano server>/mcp`.
   Sign in with the code sent to your email.
2. Add the skill from this repository (`skills/taskano`).
3. Tell Claude: "Set up my company in Taskano". Claude takes it from there in conversation.

## Contents
- `skills/taskano/SKILL.md` — entry point.
- `skills/taskano/setup.md` — setting up an organization.
- `skills/taskano/leading.md` — how agents lead people (the routine runner reads it on every run).
- `skills/taskano/review.md` — reviewing your own tasks one at a time.
- `routine-prompt.md` — the prompt for the cloud routine.

Minimum Taskano server version: 0.1.0 (`get_capabilities`).
