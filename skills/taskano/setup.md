# Setting up an organization in conversation

The goal of the first session is not "everything configured" but **one step sent to a real person**.

## 0. Where are we
Start with `get_setup_status`. Setup can be resumed: continue from the first unfinished item.

## 1. Organization
`get_setup_status` already said whether a company exists.

**It does** — an operator set it up before the first sign-in. Create nothing, go straight to the settings below.

**It does not** — Taskano is an invite-only pilot: the code comes from whoever invited the user.
`create_org(name, invite_code)`. Ask for the company name and the code in one message. No code — stop here and say
so plainly; nothing else can be set up without it.

**Either way the company starts on `en` and `UTC`** — deliberately neutral, because nobody guessed the user's
settings for them. So the first thing to settle, before people and areas, is **the company name, the interface
language and the time zone**: `update_org_settings(name, language, timezone)`, and the agent working window in the
same zone, otherwise agents keep a UTC day and every deadline lands on the wrong evening. Ask for the time zone as
a full IANA name (`Asia/Ho_Chi_Minh`, `America/Sao_Paulo`, `Europe/Berlin`); short forms such as `UTC+7` or `IST`
are rejected — `IST` alone is India, not Israel. Do not infer a country from the language the user writes in.

## 2. A conversation, not a form
Find out in plain language, with short questions, no more than two at a time:
- **Areas** of work (for example sales, support, operations). Each becomes an agent.
- **People**: name, email, preferred language, country, which area they work in,
  who should be guided step by step (guided mode, the default) and who works independently.
  The bot interface is fully localized in English, Russian, Turkish and Vietnamese. For any other language the
  person gets an English interface, while the text of their tasks is still translated for them. Say so plainly
  instead of promising more.
- **Agent working window**: the hours when agents recalculate plans (default Mon–Fri 09:00–19:00 in the
  organization's time zone).
- **Words not to translate**: brand and product names, internal terms.
- **Playbook**: how this company likes to work. Saved with `set_playbook`, read by agents on every run.

## 3. Create
- `update_org_settings` — language, time zone, window, glossary.
- `create_agent` — one per area; the owner is the person responsible for that area (by default, the user).
- `invite_person` — for each person (email, name, language). They receive an email with a link to the Telegram bot.
- `set_person_settings` — mode, quotas between areas if a person works for several (for example 60/40), and also
  `timezone` (an IANA name such as `America/Sao_Paulo`) and working hours. The bot itself offers only a few common
  time zones, so set anything else here.
- `update_org_settings(ai_budget_usd_month)` — the monthly AI budget (default $20). It limits translation and the
  analysis of work chats, not the agents' own work: when it runs out, the owner gets a notification and translation
  pauses until the budget is raised. There is no tool that reports the amount spent so far.
- `set_playbook` — the playbook.

## 4. Agent runner (routine)
Agents lead through a claude.ai cloud routine. It runs every hour during the working window, and right away when
a person has a question or is idle.

Walk the user through the steps and check each one:
1. Open https://claude.ai/code/routines → **New routine**. If the account has no Routines section, the runner
   cannot be set up: the rest of Taskano works, but agents will not lead anyone on their own. Say this plainly
   rather than looking for workarounds.
2. Prompt: the text from `routine-prompt.md` in this repository (with the organization's name filled in).
3. Repository: the user's own fork of this skill repository. Connector: Taskano (remove the others).
4. Schedule: hourly, during the organization's working window.
5. Save, open **Edit → Add another trigger → API**, copy the URL and press **Generate token**.
6. Call `connect_runner` to get a link to a Taskano page. The user opens it and pastes the URL and token there.
   **The token must never be pasted into the chat.**
7. `get_setup_status` should show the runner as connected. If it does not, work through the steps before moving on.

If the owner later gets a "cannot reach the runner" notification, the trigger token has stopped working: generate
a new one in the routine (**Edit → API trigger → Generate token**), call `connect_runner` again and paste the new
URL and token on the page it returns.

## 5. People sign in

**The owner signs in too — do not skip this.** An invitation is never sent to someone who is already a member, so
the owner links Telegram themselves: open @Taskano_bot, `/start`, sign in with the same email, enter the code from
the letter. Until that link exists Taskano cannot reach the owner at all: no decisions to make, no proposals from
agents, no "the AI budget has run out", no "cannot reach the runner", no morning digest — everything is silently
dropped. Walk the user through it and confirm it worked before promising that Taskano will tell them anything.

Each invited person gets an email with a link to the Telegram bot (@Taskano_bot). They open it, agree to the terms,
confirm their time zone, and from then on receive tasks in the bot. `get_setup_status` shows who has signed in.
People who have not signed in receive nothing — check this before promising the user that work has started.

- After signing in, the bot tells each person how to put the app on their phone's home screen, so their task list
  is one tap away instead of a search through Telegram. Repeat it to the user for their own phone: open the app
  from the bot, then "Add to Home Screen" in the "…" menu.
- The email did not arrive (spam, a blocked address): `resend_invitation(email)` issues a fresh link. `invite_person`
  also returns a `telegramLink` that can be handed over directly.
- "This Telegram is already linked to someone else": that Telegram account belongs to another Taskano user. Accounts
  are never merged. The person should sign in with the email they already use, or use another Telegram account.

## 6. Work chats (optional, any time later)
The team can add the bot to their Telegram work groups. Whoever links a group must have opened a private chat with
the bot first, otherwise the bot cannot explain what went wrong. Add @Taskano_bot to the group and make it an administrator,
otherwise Telegram hides the messages from it. Only an owner or admin of the company can link a group; they get a
confirmation in their private chat with the bot. In the group the bot stays silent. It files photos and facts from
people of the company under the right tasks, and proposes tasks from clear assignments. **Only messages from people
who are already active members of this company are read** — anything written by someone who has not signed in to
Taskano is dropped, not stored. So a group is worth linking after the team has signed in, not before, otherwise it
looks as if the bot collects nothing. One group belongs to one company at a time. `list_groups` shows linked
groups, `pause_group` / `resume_group` turn the intake off and on.

## 7. Running it afterwards
- An area is on hold (seasonal, the person is away): `update_agent(paused: true)`, and `paused: false` to resume.
  A paused agent leads no one and is skipped in the briefing.
- Someone leaves the company: `offboard_person`. Their open tasks are cancelled and their timers stop.
- Someone wants a different language or working hours: `set_person_settings`. People can also switch the bot's
  language themselves with `/language`.
- Check on people any time: `review_person`, `list_tasks`, `agent_metrics`.

## 8. First result
- `create_plan` for one real piece of work for one person, 2–3 steps (`add_step`), then `release_step` for the first.
- Tell the user: the person will get the step in Telegram as soon as they sign in from the invitation email.
- The session ends when the step is sent. `get_setup_status` shows that the first task exists.
