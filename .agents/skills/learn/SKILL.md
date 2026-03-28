---
name: learn
description: Use only when the user explicitly asks to capture a lesson, extract a reusable pattern, save a learning, reflect on how to improve, or turn recent work into a learned skill or a human improvement item.
---

# Learn

Treat the current repository root as the path base for project-local files.

## Purpose

Capture explicit learning from recent work without relying on automatic hooks.

This skill supports two learning outputs:

- a project-local learned skill under `.work/catalog/learned/skills/`
- a private human improvement item under `.work/human-learning/items/`

Use this skill only when the user explicitly asks to learn, capture a lesson, save a pattern, or reflect on personal improvement.

## Output Verdicts

Choose exactly one verdict before writing:

- `save-skill`
- `save-human-item`
- `both`
- `drop`

## Workflow

1. Identify the learning source:
   - the current task file
   - the recent task outcome
   - the current conversation
   - explicit user notes
2. Determine what is worth preserving:
   - a reusable technical or workflow pattern for future project work
   - a human improvement topic about alignment, blockers, or better collaboration behavior
   - both
3. Inspect overlap before drafting:
   - `.work/catalog/learned/skills/`
   - `.work/catalog/validated/skills/`
   - `.agents/skills/`
   - `.work/human-learning/items/` when a human item is plausible
4. Decide the verdict:
   - `save-skill` for a reusable project pattern
   - `save-human-item` for a private human improvement topic
   - `both` when both outputs are genuinely useful
   - `drop` when the lesson is trivial, one-off, or already covered
5. For a learned skill:
   - create or update `.work/catalog/learned/skills/<id>/`
   - write `SKILL.md`
   - write `catalog.yaml`
   - keep it in the `learned` layer
   - do not auto-install it into `.agents/skills/` unless the user explicitly asks
6. For a human item:
   - create or update `.work/human-learning/items/<id>.md`
   - use YAML frontmatter with:
     - `id`
     - `status`
     - `created`
     - `updated`
     - `success_streak`
     - `retire_after`
     - `tags`
   - default:
     - `status: active`
     - `success_streak: 0`
     - `retire_after: 3`
   - use section order:
     - `# Title`
     - `## Why This Matters`
     - `## Applies When`
     - `## Better Behavior`
     - `## Success Signals`
     - `## Failure Signals`
     - `## Notes`
7. If there is clear overlap:
   - prefer updating the existing learned skill or human item
   - do not create duplicates
8. Prepare a brief preview:
   - verdict
   - target paths
   - whether this is a new item or an update
   - the core lesson being preserved
9. Ask for confirmation.
10. After confirmation, write only the needed changes.

## Writing Rules

- Do not rely on automatic hook-based observation.
- Keep learned skills focused on one reusable pattern.
- Keep human items focused on one improvement topic.
- Human items are private by default.
- Do not create or update project runtime config from this skill.
- Do not install a learned skill unless the user explicitly asks.
- If the right answer is `drop`, explain why and do not write files.
