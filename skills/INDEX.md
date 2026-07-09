# Skills Index

## 30-Second Start

- If the user is asking what `it-runner` is, why it exists, or when it should be used, start with `skills/what-is-it-runner/SKILL.md`.
- If the problem is about `.it-runner` tasks, task visibility, logs, API, `envs-next`, or `.STATE` control, start with `skills/it-runner-workflow/SKILL.md`.
- If the work is happening inside Agentflow, also read `skills/it-runner-workflow/references/agentflow-hosted-mode.md` before changing runner lifecycle assumptions, `runtime.json`, or hosted registration behavior.
- If the problem is about `agentd.exe`, `controlapi`, `agentctl`, remote Windows task application, or `agent-task.yaml`, start with `skills/it-runner-agentd-control/SKILL.md`.
- If the problem is specifically about `.it-runner` env layering and naming, also read `skills/it-runner-workflow/references/env-conventions.md`.
- If the problem is choosing the right `.it-runner` task model, also read `skills/it-runner-workflow/references/task-centric-patterns.md`.
- If the problem is upgrading a legacy `.it-runner` project to the new env conventions, start with `skills/it-runner-convention-upgrade/SKILL.md`.
- If you just need commands, read `skills/IT-RUNNER-CHEATSHEET.md`.
- If you want a folder/file starting point, read `skills/TEMPLATES.md`.

This folder contains reusable skills for engineering `it-runner` workflows,
Agentflow-hosted runner usage, `.it-runner` env conventions, and remote agent
control.

## Recommended Order

1. Use `what-is-it-runner` when the user needs concept or adoption guidance.
2. Use `it-runner-workflow` for `.it-runner` structure, task authoring, API debugging, logs, state files, or runner behavior.
3. Use `agentflow-hosted-mode.md` when runner lifecycle assumptions involve Agentflow hosting.
4. Use `it-runner-convention-upgrade` when an existing project must migrate from old env naming/layout to the strict numbered convention.
5. Use `it-runner-agentd-control` when the task crosses into remote Windows agent control, package publication, or `agentctl`-driven operations.
6. Use `task-centric-patterns.md` to choose a task-centered `.it-runner` model.
7. Use `project-rollout-status.md` only as historical reference for known migrated repos and templates.

## Skill Selection

### `what-is-it-runner`
- Path: `skills/what-is-it-runner/SKILL.md`
- Use for: explaining what `it-runner` is, why it exists, how it fits with Agentflow or AI agents, and when a workflow should become a durable task

### `it-runner-workflow`
- Path: `skills/it-runner-workflow/SKILL.md`
- Use for: `.it-runner` authoring, task discovery, API debugging, logs, state file control, env resolution, and runner fixes
- Companion: `skills/it-runner-workflow/references/agentflow-hosted-mode.md` when the runner is hosted by Agentflow

### `it-runner-agentd-control`
- Path: `skills/it-runner-agentd-control/SKILL.md`
- Use for: `agentd.exe`, `controlapi`, `agentctl`, `agent-task.yaml`, remote Windows task apply/restart/status/logs, and reusable winagent task templates

### `it-runner-convention-upgrade`
- Path: `skills/it-runner-convention-upgrade/SKILL.md`
- Use for: upgrading legacy `.it-runner` env layouts using the new checker and numbered env rules

## Sequencing Patterns

### User Is Still Learning The Concept
- `what-is-it-runner`
- `it-runner-workflow` after the user moves from concept to authoring/debugging

### Broken Or Missing Tasks
- `it-runner-workflow`
- `agentflow-hosted-mode.md` if the work happens inside Agentflow or touches hosted runtime assumptions

### Existing Project With Legacy Env Layout
- `it-runner-convention-upgrade`
- `it-runner-workflow` if task behavior must be revalidated after migration

### Remote Windows Agent Control
- `it-runner-agentd-control`
- `it-runner-workflow` if the local `.it-runner` task structure or env layout is also broken

## Trigger Cheat Sheet

- Quick trigger reference: `skills/TRIGGERS.md`
- Minimal template checklist: `skills/TEMPLATES.md`
- It-runner cheatsheet: `skills/IT-RUNNER-CHEATSHEET.md`
- It-runner task model patterns: `skills/it-runner-workflow/references/task-centric-patterns.md`
- What it-runner is: `skills/what-is-it-runner/SKILL.md`
- It-runner Agentflow hosted mode: `skills/it-runner-workflow/references/agentflow-hosted-mode.md`
- It-runner task reading and naming: `skills/it-runner-workflow/references/task-reading-and-naming.md`
- It-runner log reading and paths: `skills/it-runner-workflow/references/log-reading-and-paths.md`
- It-runner UI semantics: `skills/it-runner-workflow/references/ui-semantics.md`
- It-runner rollout status: `skills/it-runner-workflow/references/project-rollout-status.md`
- It-runner convention upgrade: `skills/it-runner-convention-upgrade/SKILL.md`
- Navigation map: `skills/NAVIGATION.md`
- Reading order: `skills/READING-ORDER.md`
