# Codex Project Root Kit

A private-first, single-project Codex operating root with explicit workflow skills, a layered capability catalog, temporary staging, durable task records, and private human learning.

## Use

This repository is the source for a reusable Codex project root.

The main working loop is intentionally small:

1. Use `$project-manage` when you need to bootstrap, inspect, repair, or change the runtime layer or capability catalog.
2. Use `$open-task` when you start or resume real work.
3. Use `$close-task` when you stop work and want the task record updated cleanly.
4. Use `$learn` only when you explicitly want to preserve a reusable lesson or a human improvement item.

### Core skills

| Skill | Use it for | Recommendation |
| --- | --- | --- |
| `$project-manage` | Runtime layer changes, catalog inspection, capability install/promote/remove, MCP/config management | Keep it focused on runtime and catalog management |
| `$open-task` | Start or resume a task record in `.work/tasks/` | Use at the start of real work, especially after context switches |
| `$close-task` | Update task outcome, next step, closure state, and relevant human-learning items | Use every time you stop work so the project stays resumable |
| `$learn` | Capture a learned skill or a private human improvement item | Use explicitly and selectively; not every task deserves a learned artifact |

### Practical guidance

- Keep the shipped skills manual-only and call them explicitly.
- Use `.tmp/` as a neutral staging area for temporary source material you want Codex to process.
- When the right destination for staged material is unclear, ask the model to suggest the best durable destination before writing.
- Treat task files as the main durable record of active work.
- Treat catalog items as reusable capabilities, not task notes.
- Treat `.work/human-learning/items/` as a small active improvement queue, not a dumping ground.

### Git behavior

This repository tracks `.agents/`, `.codex/`, `.work/catalog/`, and the structural parts of `.work/human-learning/` because those files define the product.

In a real project checkout that uses this baseline, the same paths should usually stay local-only. Do not put them in the team repo's tracked `.gitignore` by default. Instead, copy the patterns from [project-local-exclude.example](/home/duu/code/mycodex/workspace/project-local-exclude.example) into `.git/info/exclude` in the real project checkout.

This repository also keeps runtime noise out of version control:

- staged files in `.tmp/` are ignored except for the directory's own tracked guidance files
- task files in `.work/tasks/` are ignored except for the directory's tracked ignore file
- human-learning item files in `.work/human-learning/items/` are ignored except for the directory's own tracked guidance files

## Design

### Structure

```text
workspace/
├── .tmp/
├── .agents/skills/
├── .codex/
├── .work/
│   ├── catalog/
│   ├── human-learning/
│   └── tasks/
├── AGENTS.md
├── README.md
└── project-local-exclude.example
```

- `.agents/skills/` contains the active workflow skills and any active project skills
- `.codex/` contains active project config, subagents, and rules
- `.work/catalog/` contains the private layered capability catalog
- `.work/tasks/` contains durable task records
- `.work/human-learning/` contains private human improvement items
- `.tmp/` is a neutral staging area for temporary source material

### Core ideas

The design is built around a few constraints:

- Single-project root: each runtime root is for one project, not a multi-project control plane.
- Explicit workflow: the main skills are called intentionally, not implicitly.
- Runtime vs private state: active Codex runtime files live at the root, while private task, catalog, and human-learning material lives under `.work/`.
- Layered capability trust: catalog items move through `external`, `learned`, and `validated`.
- Human improvement stays private: human-learning belongs under `.work/`, not in the runtime layer.
- Temporary staging, not accidental memory: `.tmp/` exists so temporary source material has a clear place and a clear cleanup rule.

### Why the git setup is split

There are two different use cases:

1. This repository is the source of the baseline, so its baseline files should be committed here.
2. A real project checkout uses the same structure as local operating state, so those files should usually stay uncommitted there.

That is why this repository tracks the baseline itself, but the recommended setup for real projects is local ignore through `.git/info/exclude`.
