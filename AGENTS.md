# Single-Project Codex Guide

## Scope

- This directory is the maintained, reusable single-project Codex root.
- Treat the root as one project workspace, not as a multi-project control plane.

## Instruction Boundaries

- `README.md` files are human-facing.
- `AGENTS.md` files are model-facing.
- Do not invent generic docs, templates, plans, registries, or archive folders for this baseline.

## Root Layout

- `AGENTS.md` is the root instruction file for this project.
- `.tmp/` stores temporary user-provided files waiting to be processed.
- `.agents/skills/` stores active project skills.
- `.codex/config.toml` stores active project configuration.
- `.codex/agents/` stores active project subagents.
- `.codex/rules/` stores optional command execution rules.
- `.work/catalog/` stores the private layered capability catalog and active inventory.
- `.work/human-learning/` stores private human improvement material.
- `.work/tasks/` stores private task records.

## Workflow Surface

- Prefer these skills as the primary interface:
  - `$project-manage`
  - `$open-task`
  - `$close-task`
- `$learn` is an optional manual learning skill.
- These shipped workflow skills are manual-only and should not be implicitly invoked.
- Use `$learn` only when the user explicitly asks to capture learning or reflection.
- Treat the files as the durable source of truth.
- Treat the skills as guided entrypoints that collect missing information, preview changes, and then write.

## Temporary Staging Contract

- `.tmp/` is an explicit staging area for files the user wants the model to process.
- Do not treat `.tmp/` as durable project knowledge.
- Do not proactively scan or load `.tmp/` unless the user points to it or asks to organize staged material.
- `.tmp/` is not owned by any single skill.
- When the user references staged material, first determine or confirm the best durable destination if it is not obvious.
- When staged material is processed successfully into a durable destination, prefer cleaning up the processed original from `.tmp/` after preview and confirmation.
- If the destination is unclear, the transformation is incomplete, or the user has not confirmed cleanup, keep the original staged file.

## Task Record Contract

- Path: `.work/tasks/YYYY-MM-DD-task-slug.md`
- Metadata: YAML frontmatter with:
  - `status`
  - `created`
  - `updated`
  - `closed`
  - `tags`
- Allowed `status` values:
  - `planned`
  - `active`
  - `blocked`
  - `done`
  - `dropped`
- Section order:
  - `# Title`
  - `## Goal`
  - `## Out of Scope` when boundary control matters
  - `## Inputs / Constraints`
  - `## Relevant Paths`
  - `## Current State`
  - `## Active Decisions`
  - `## Open Issues`
  - `## Next Step`
  - `## Final Outcome`
  - `## Important Events`

## Task Writing Rules

- Task files are the only durable dynamic record in this baseline.
- Do not create separate state files, plan files, generic docs, or template files.
- Keep `Current State`, `Open Issues`, and `Next Step` useful for resume.
- Keep `Final Outcome` empty until the task is closed as `done` or `dropped`.
- Use `Important Events` only for meaningful turning points, not a full session diary.
- Update `updated` on every meaningful task edit.
- Set `closed` only when `status` becomes `done` or `dropped`.
- Closed tasks remain in `.work/tasks/`; do not move them elsewhere.

## Catalog Contract

- `.work/catalog/active.yaml` tracks catalog-managed active capabilities only.
- `active.yaml` items should record:
  - `id`
  - `kind`
  - `source_layer`
  - `source_ref`
  - `installed_paths`
  - `installed_at`
  - `managed`
  - `notes`
- Layer order and meaning:
  - `external`: imported or collected candidate items
  - `learned`: items created or learned from project work
  - `validated`: items personally proven useful in practice
- Each layer contains:
  - `skills/`
  - `agents/`
  - `mcp/`
  - `config/`
  - `packs/`
- Skill items keep a full skill directory shape.
- Agent items keep `agent.toml` plus `catalog.yaml`.
- MCP items keep `server.toml` plus `catalog.yaml`.
- Config items keep `catalog.yaml` plus material to install into `.codex/config.toml` or `.codex/rules/`.
- Pack items keep `pack.yaml` and reference other catalog items by id and kind.
- `catalog.yaml` should record:
  - `id`
  - `kind`
  - `title`
  - `summary`
  - `layer`
  - `origin`
  - `source_ref`
  - `dependencies`
  - `validated`
  - `notes`
- `pack.yaml` should record:
  - `id`
  - `title`
  - `summary`
  - `layer`
  - `origin`
  - `source_ref`
  - `validated`
  - `includes`
  - `warnings`
  - `dependencies`
  - `notes`

## Capability Rules

- Keep active runtime capabilities in standard root paths:
  - skills under `.agents/skills/`
  - subagents under `.codex/agents/`
  - config in `.codex/config.toml`
  - rules under `.codex/rules/`
- `project-manage` is the only project-level management entrypoint.
- Install may come from any catalog layer, but items outside `validated` must be treated as not yet trusted.
- Prefer materializing active items into the standard root paths.
- Install MCP items into `.codex/config.toml` as managed `[mcp_servers.<id>]` blocks.
- When editing `.codex/config.toml`, use managed blocks for catalog-installed config snippets.
- Keep this operating layer private by default unless the user explicitly asks to prepare shared versions.
- Human-learning items live under `.work/human-learning/`. They are private working material, not active project capabilities.
- Use `.tmp/` as the preferred local staging area for temporary user-provided source material that needs to be organized into a durable destination.

## Done Means

- The root remains a minimal single-project baseline.
- The workflow skills match the documented workflow.
- The catalog and active inventory stay aligned with the materialized runtime layer.
- Task records stay compact, readable, and easy to resume or review later.
