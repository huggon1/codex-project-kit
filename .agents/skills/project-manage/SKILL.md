---
name: project-manage
description: Use when the user wants to initialize, inspect, repair, or change the active Codex operating layer and layered catalog for the current project. Trigger on requests like project setup, project bootstrap, inspect project config, inspect catalog, add skill, add subagent, install pack, promote item, change config, or repair project layer.
---

# Project Manage

Treat the current repository root as the path base for all files mentioned below.

## Purpose

Manage the active Codex operating layer and layered catalog for the current project.

## Required Outcomes

After a bootstrap or repair run, these should exist:

- `AGENTS.md`
- `.agents/skills/`
- `.codex/config.toml`
- `.codex/agents/`
- `.codex/rules/`
- `.work/catalog/active.yaml`
- `.work/catalog/external/`
- `.work/catalog/learned/`
- `.work/catalog/validated/`
- `.work/tasks/`

## Workflow

1. Determine the user goal:
   - bootstrap or repair the project layer
   - inspect the layered catalog or active inventory
   - inspect current active capabilities
   - add, remove, or update active skills
   - add, remove, or update active subagents
   - add, remove, enable, disable, or inspect MCP servers
   - add, remove, or update active config or rules
   - install a pack
   - promote an item between layers
   - import a local source into `external`
2. Inspect the current root before asking obvious questions:
   - `AGENTS.md`
   - `.agents/skills/`
   - `.codex/config.toml`
   - `.codex/agents/`
   - `.codex/rules/`
   - `.work/catalog/active.yaml`
   - `.work/catalog/external/`
   - `.work/catalog/learned/`
   - `.work/catalog/validated/`
   - `.work/tasks/`
3. For catalog inspection, prefer explaining:
   - current active managed items from `active.yaml`
   - available items by layer
   - dependencies, warnings, and validation state from item metadata
   - inventory structure:
     - `id`
     - `kind`
     - `source_layer`
     - `source_ref`
     - `installed_paths`
     - `installed_at`
     - `managed`
     - `notes`
4. For installation:
   - allow install from any layer
   - warn clearly when the source layer is `external` or `learned`
   - materialize active skills into `.agents/skills/`
   - materialize active subagents into `.codex/agents/`
   - materialize MCP items into `.codex/config.toml` as managed `[mcp_servers.<id>]` blocks
   - materialize config snippets into `.codex/config.toml` using managed blocks
   - materialize rule files into `.codex/rules/`
   - update `.work/catalog/active.yaml`
5. For pack installation:
   - read `pack.yaml`
   - inspect included item ids, dependencies, and warnings
   - preview the full install set before writing
6. For agent installation:
   - ensure the agent is usable after install
   - prefer a matching catalog config item when one exists
   - otherwise add the smallest managed config block needed to register the subagent and record that in `active.yaml`
7. For removal:
   - use `active.yaml` as the primary source of truth
   - remove only the installed paths or managed config blocks associated with that item
   - update `active.yaml`
8. For MCP inspection:
   - explain the installed server id
   - explain the command or URL source
   - explain required env or other prerequisites if metadata records them
   - explain whether the item is enabled through the current managed block
9. For promotion:
   - move the item directory between `external`, `learned`, and `validated`
   - update the item's metadata to match the new layer and validation state
   - preserve provenance in `origin`, `source_ref`, and notes
10. For repair:
   - compare `active.yaml` with active runtime files
   - explain the drift
   - preview the smallest repair
11. If the user names a local capability source path, inspect it before asking for more detail.
12. Prepare a brief preview of the files or directories to create, update, install, remove, promote, or repair.
13. Ask for confirmation.
14. After confirmation, write only the needed changes.

## Writing Rules

- Preview before writing.
- Keep all changes local to the current project root.
- Keep the catalog layer private under `.work/catalog/`.
- Prefer materializing active capabilities into `.agents/skills/`, `.codex/agents/`, `.codex/config.toml`, and `.codex/rules/`.
- Use `.work/catalog/active.yaml` for catalog-managed active items only; do not track the baseline workflow skills there.
- When writing `active.yaml`, use the fields:
  - `id`
  - `kind`
  - `source_layer`
  - `source_ref`
  - `installed_paths`
  - `installed_at`
  - `managed`
  - `notes`
- Use config block markers in `.codex/config.toml`:
  - `# project-manage: begin <item-id>`
  - `# project-manage: end <item-id>`
- For MCP items, the managed block should contain one `[mcp_servers.<id>]` definition sourced from `server.toml`.
- Do not assume a global catalog exists.
- Do not create generic docs, templates, plan files, registries, or archive folders.
- Keep changes minimal and deterministic.
