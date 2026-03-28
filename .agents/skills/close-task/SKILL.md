---
name: close-task
description: Use when the user wants to stop work on a task, end a session, finish a task, or drop a task. Trigger on requests like close task, end session, stop work, finish task, or complete task.
---

# Close Task

Treat the current repository root as the path base for all files mentioned below.

## Purpose

Close out the current work state for a task, whether the task remains live or becomes closed.

This skill may also do a light review against existing human-learning items under `.work/human-learning/items/` when that directory exists.

## Workflow

1. Identify the task file.
   - if the user gives it directly, use it
   - otherwise search `.work/tasks/`
   - if multiple candidates are plausible, ask
2. Read the task file.
3. Collect the session outcome:
   - current state
   - active decisions
   - open issues
   - next step
   - meaningful event text
   - target status:
     - `planned`
     - `active`
     - `blocked`
     - `done`
     - `dropped`
   - prefer:
     - `active` when work should continue next time
     - `blocked` when progress depends on an unresolved blocker
     - `done` when the task is complete
     - `dropped` when the task is intentionally abandoned
     - `planned` only when the task is being parked before active work resumes
4. If `.work/human-learning/items/` exists:
   - inspect only `status: active` items
   - consider only items clearly relevant to the task that is being closed
   - classify each relevant item as:
     - `met`
     - `missed`
     - `not-applicable`
   - for `met`:
     - increment `success_streak`
     - update `updated`
   - for `missed`:
     - reset `success_streak` to `0`
     - update `updated`
   - if `success_streak` reaches or exceeds `retire_after`, ask whether to set `status: delete`
   - do not create new human-learning items; use `$learn` for that
5. Prepare a brief preview:
   - fields to update
   - any human-learning item updates
   - whether `Final Outcome` and `closed` will be written
6. Ask for confirmation.
7. After confirmation:
   - update the task file
   - update any confirmed human-learning items
   - append one concise important event when meaningful
   - if status is `done` or `dropped`, write `Final Outcome` and set `closed`
   - otherwise keep `Final Outcome` empty and `closed` unset
   - keep the file in `.work/tasks/`

## Writing Rules

- Preview before writing.
- Keep `Important Events` concise and append-only.
- Do not create an archive move; closed tasks remain in `.work/tasks/`.
- Keep human-learning review brief and relevant.
- Only update existing human-learning items from this skill.
- Never delete human-learning files from this skill.
- When a human-learning item is no longer needed, set `status: delete` instead of removing it.
- Return the final task path after the update.
