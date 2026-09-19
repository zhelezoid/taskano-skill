# Task review — one at a time, five steps

Starts when the user says "let's go through my tasks", "task review", "let's review what's on my plate" or similar,
in any language. The goal is not to retell the list but to bring each task to a recorded decision.

## Preparation (once, at the start)

1. Collect the tasks: `personal_list` (personal) and `list_tasks` for each company with `person` = the user (open
   tasks where they are the assignee or the human author). Include waitings ("waiting on others").
2. Show them as **one short list** in two blocks — **Personal** and **Company <name>** — titles only, one line
   each. Review order: ★ important first, then "decide", then the ones that have been sitting longest.
3. Ask one thing: "Start with <first task>, or pick another?" — then go one by one.

## One task — five steps, strictly one step per message

Do not give all the steps at once. Each step is a short message with one question; the next step comes after the
answer.

1. **Discuss.** Recall the gist in 2–3 lines (from the description, comments, attachments) and ask what has changed
   since it was recorded. Listen; clarify with facts, not opinions.
2. **Reflect.** Help the user see the task honestly: what is really in the way, what they are avoiding, what "doing
   nothing for another month" would cost. One or two observations, no judgement and no pep talk.
3. **Record the decision.** Put the decision in one sentence ("no second location until orders double") and ask
   "right?". After a "yes" — `comment_task(text: "Decision: …")`.
4. **Strategy.** How exactly it gets done: 2–4 steps, who, by when, what could go wrong. Once agreed —
   `comment_task(text: "Strategy: …")`.
5. **Commit.** Bring the system in line with the decision and say what you did in one line:
   - done / no longer needed → `personal_done` or `cancel_task`;
   - the kind or importance changes → `personal_update` (`category`, `important`, `due_date`);
   - waiting on someone → `personal_update(waiting_for, check_date)`;
   - strategy steps done by another person → company tasks (`create_task` with `why`, `done_criteria`,
     `due_date`); the user's own steps → `personal_add`.

Then: "Next — <title>?". The user can say "stop" at any time — then give a short summary: how many tasks were
reviewed and which decisions were recorded.

## Rules

- One task at a time, one step per message. Do not move to the next task until the commit step is done.
- Decisions and strategies are recorded only after an explicit "yes".
- Delete nothing without an explicit "yes".
- Personal and company never mix: tasks for employees go to the company, the user's own items go to personal.
