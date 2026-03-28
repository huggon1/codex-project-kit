---
name: open-task
description: Use when the user wants to start a new task or resume an existing task for the current project. Trigger on requests like open task, start task, continue task, resume task, start session, or resume session.
---

# Open Task

Treat the current repository root as the path base for all files mentioned below.

## Purpose

Open the right task record for work, either by creating a new task file or by resuming an existing one.

## Preconditions

The project operating layer must already exist, including `.work/tasks/`.

If it does not exist:

- stop
- tell the user to run `project-manage` first

## Workflow

1. Verify that `.work/tasks/` exists.
2. Decide whether this is a new task or an existing one:
   - search `.work/tasks/` for likely matches
   - if there is a clear existing task, prefer resume
   - if there are multiple plausible candidates, ask
   - if there is no good candidate, create a new task
3. For a new task:
   - collect title, goal, out of scope when needed, inputs, constraints, relevant paths, current state, next step, and optional tags
   - propose a filename using `YYYY-MM-DD-task-slug.md`
   - create YAML frontmatter with:
     - `status`
     - `created`
     - `updated`
     - `closed`
     - `tags`
   - default `status` to `active` when the user is starting work now
   - use `planned` only when the user explicitly wants to park the task before active work starts
   - leave `closed` empty on creation
   - build the task file with these sections in order:
     - `# Title`
     - `## Goal`
     - `## Out of Scope` when needed
     - `## Inputs / Constraints`
     - `## Relevant Paths`
     - `## Current State`
     - `## Active Decisions`
     - `## Open Issues`
     - `## Next Step`
     - `## Final Outcome`
     - `## Important Events`
4. For an existing task:
   - read the task file
   - summarize the current state, open issues, next step, and status
   - only prepare a file update if something needs to change
5. Prepare a brief preview:
   - task path
   - create vs resume
   - the most important fields to write or update
6. Ask for confirmation.
7. After confirmation, write only the needed changes.

## Writing Rules

- Preview before writing.
- Do not create a duplicate task if a matching active task already exists.
- Prefer minimal updates when resuming an existing task.
- If no task-file change is needed, do not rewrite the file.
- Leave `Final Outcome` empty until the task is closed as `done` or `dropped`.
- Keep `Important Events` limited to meaningful turning points, not a full session log.
- After success, point the user to the reading order:
  - `AGENTS.md`
  - the task file
  - deeper project files only if needed
