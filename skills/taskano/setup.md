# Setting up an organization in conversation

The goal of the first session is not "everything configured" but **one step sent to a real person**.

## 0. Where are we
Start with `get_setup_status`. Setup can be resumed: continue from the first unfinished item.

## 1. Organization
No company yet → a Taskano invite code is needed (issued by the Taskano operator). `create_org(name, invite_code)`.
Ask for the name and the code in one message.

## 2. A conversation, not a form
Find out in plain language, with short questions, no more than two at a time:
- **Areas** of work (for example sales, support, operations). Each becomes an agent.
- **People**: name, email, preferred language, country, which area they work in,
  who should be guided step by step (guided mode, the default) and who works independently.
- **Agent working window**: the hours when agents recalculate plans (default Mon–Fri 09:00–19:00 in the
  organization's time zone).
- **Words not to translate**: brand and product names, internal terms.
- **Playbook**: how this company likes to work. Saved with `set_playbook`, read by agents on every run.

## 3. Create
- `update_org_settings` — language, time zone, window, glossary.
- `create_agent` — one per area; the owner is the person responsible for that area (by default, the user).
- `invite_person` — for each person (email, name, language). They receive an email with a link to the Telegram bot.
- `set_person_settings` — mode, and quotas between areas if a person works for several (for example 60/40).
- `set_playbook` — the playbook.

## 4. Agent runner (routine)
Agents lead through a claude.ai cloud routine. It runs every hour during the working window, and right away when
a person has a question or is idle.

Walk the user through the steps and check each one:
1. Open https://claude.ai/code/routines → **New routine**.
2. Prompt: the text from `routine-prompt.md` in this repository (with the organization's name filled in).
3. Repository: this skill repository. Connector: Taskano (remove the others).
4. Schedule: hourly, during the organization's working window.
5. Save, open **Edit → Add another trigger → API**, copy the URL and press **Generate token**.
6. Call `connect_runner` to get a link to a Taskano page. The user opens it and pastes the URL and token there.
   **The token must never be pasted into the chat.**
7. `get_setup_status` should show the runner as connected. If it does not, work through the steps before moving on.

## 5. First result
- `create_plan` for one real piece of work for one person, 2–3 steps (`add_step`), then `release_step` for the first.
- Tell the user: the person will get the step in Telegram as soon as they sign in from the invitation email.
- The session ends when the step is sent. `get_setup_status` shows that the first task exists.
