---
name: it-runner-workflow
description: Use when creating, debugging, or refactoring `.it-runner` projects and tasks, inspecting `it-runner` APIs, troubleshooting task discovery and env expansion issues, or making targeted fixes in the `it-runner` codebase itself.
---

# It-Runner Workflow

Use this skill for everything related to `.it-runner` structure, task authoring, API debugging, and `it-runner` runtime troubleshooting.

## Agentflow Hosted Mode

When this skill is used inside Agentflow, the default assumption is that `it-runner`
is hosted by Agentflow through `it-runner-agentflow --mode agentflow`.

Important boundaries:

- Most tasks should focus on `.it-runner` project/task authoring, task discovery,
  logs, APIs, and task behavior.
- Do not casually change the host runtime model just because Agentflow is using
  `it-runner`.
- Do not infer that Agentflow-hosted `it-runner` should run through
  `--runner-config it-runner.yaml`.
- Do not rewrite `data/it-runner/runtime.json` using assumptions from the original
  standalone runner mode.

If the problem involves Agentflow settings-page lifecycle, host registration,
`runtime.json`, host token, or re-register / hosted-runtime semantics, also read
`references/agentflow-hosted-mode.md`.

If the task is specifically about remote Windows program control through `agentd.exe`, `controlapi`, `agentctl`, or `agent-task.yaml`, use `../it-runner-agentd-control/SKILL.md` first, then return here for generic `.it-runner` issues.

If the main task is specifically migrating a legacy project from old env naming to the new numbered env convention, prefer `it-runner-convention-upgrade` first, then come back here for runtime verification.

## Goal

Create `.it-runner` setups that are discoverable, debuggable, and stable across business repos and `ops-fleet`.

## What This Skill Covers

- project-level `.it-runner` layout
- `project.yaml` conventions
- `task.yaml` conventions
- env file loading and debugging
- task visibility and task discovery failures
- API-based debugging of tasks
- targeted `it-runner` core fixes when behavior is clearly in the runner itself
- Agentflow-hosted mode boundaries for when task-level knowledge applies and when
  host-runtime assumptions do not

## Required Directory Model

At minimum, expect:

```text
.it-runner/
  project.yaml
  envs/
    000-defaults.env
    010-local.env
  tasks/
    <task-name>/task.yaml
  logs/
  states/
```

Important rule: tasks should normally be discovered as **task directories containing `task.yaml`**, not loose YAML files.

## Project Rules

- `project.yaml` is the discovery root.
- Prefer `${PROJECT_ROOT}` when composing paths.
- Keep `logsDir`, `tasksDir`, and `envFiles` explicit when needed.
- Be careful with env expansion order when using values like `${DATA_ROOT}`.

## Task Rules

- Every task must include `version: "1"`.
- Prefer a small, intentional task surface.
- Use clear names that reflect operator intent.
- For families of tasks, prefer selected-target tasks over excessive duplication.
- For newly authored tasks, include `watch.stateFile` by default so agents and
  operators can restart/stop the task through `STATE`; omit it only for a
  deliberate one-shot task that must not support file-based control.
- Prefer `watch.stateFile` over deprecated `watch.stopFile` /
  `watch.restartFiles`.
- Prefer task-local env plans over pushing task-specific parameters into project-wide `010-local.env`.

## Task-Centric Design Rule

When the repo has more than trivial tasks, prefer task-centered env assembly:

- shared project defaults stay in `.it-runner/envs/000-defaults.env`
- machine-local overrides stay in `.it-runner/envs/010-local.env`
- task defaults move into `tasks/<task>/envs/000-defaults.env`
- reusable parameter sets move into `.it-runner/envsets/`
- `task.yaml` should declare `env.autoDirs`, `env.includeSets`, and `env.required` whenever a task has real parameter needs

Do not assume one universal pattern. Choose the smallest viable task-centric pattern for the repo.

## State File Control

`watch.stateFile` is opt-in at the task YAML level. If the field is missing,
writing `STATE` commands cannot restart or stop that task. When creating or
reviewing a task that is expected to be repairable by an AI agent, add an
explicit state file path such as:

```yaml
watch:
  stateFile: ${PROJECT_ROOT}/.it-runner/states/<task-name>.STATE
```

When a task defines `watch.stateFile`, there are two supported ways to trigger it:

1. Use the HTTP API
2. Write a control command into the task's `.STATE` file

Recommended file-based control commands are:

- `echo "RESTART $(date +%s)" > <STATE_FILE>`
- `echo "STOP $(date +%s)" > <STATE_FILE>`

Important notes:

- Include a changing token such as a timestamp so each command is treated as new.
- The recommended control words are `RESTART` and `STOP`.
- Existing commands are primed at runner startup, so write a fresh command after
  the runner is already watching the task.
- Do not assume arbitrary text has meaningful semantics; prefer the runner's documented control format.
- A plain write that changes the file may still trigger behavior in some setups, but standardize on `RESTART <token>` / `STOP <token>`.

## API Debugging Workflow

When a task is missing or failing, use this sequence:

1. Confirm the task file exists in the right directory structure.
2. Confirm the task appears in the task listing API.
3. Inspect `envs-next` to see the next resolved environment.
4. Trigger the task via API.
5. Inspect task status and logs.
6. Only after that decide whether the bug is in task config, env files, or `it-runner` itself.

## Agent Fix / Restart / Verify Loop

When an AI agent is asked to fix a failing it-runner task, do not stop after
editing code or config. Use the runner as the verification harness:

1. Read the current task state and logs.
   - Start with the task API state, especially `status`, `running`,
     `lastError`, `logDir`, `runId`, and process metadata.
   - Read `it-runner.log` for runner-side failures such as env expansion,
     cleanup, start commands, port readiness, HTTP probes, and restart history.
   - Read task stdout/stderr for application, build, test, or dependency errors.
2. Identify the most likely root cause.
   - Distinguish task config/env problems from application code failures and
     runner bugs.
   - Do not rewrite unrelated task structure while investigating one failure.
3. Make the smallest relevant fix.
   - Patch application code, task YAML, project config, or env defaults according
     to the evidence.
   - Preserve local secrets and machine-local overrides.
4. Restart the task.
   - Prefer `POST /it-runner/api/tasks/{taskKey}/restart` when task keys and the
     API are available.
   - If the task defines `watch.stateFile`, writing
     `RESTART <fresh-token>` to that file is also valid.
5. Re-read the latest run.
   - Do not rely on logs from the previous run.
   - Re-fetch task state and logs after restart.
   - Prefer the stable `latest/` path when reading files directly.
6. Decide from evidence whether the fix worked.
   - Success can mean `status=done`, a service is running and probes passed, or
     the target build/test command passed.
   - If it still fails, repeat the loop with the new logs. Keep the loop bounded;
     after about three failed repair attempts, stop and report the remaining
     evidence and the decision point.

## Common Failure Modes

- `task.version missing`
- task file exists but is not discovered because it is not in `<task-dir>/task.yaml` form
- env values missing from `envs-next`
- `logsDir` or `tasksDir` expands incorrectly due to env expansion timing
- generated task includes point at missing include files
- UI caches older task lists and needs project reload

## When To Patch `it-runner` Itself

Only patch `it-runner` when:
- the task file is valid
- API discovery shows mismatched or missing behavior inconsistent with config
- `envs-next` or task execution proves the runner is transforming config incorrectly
- you can explain the bug in terms of `project.yaml`, task discovery, env expansion, or API behavior

When patching `it-runner`, add a small regression test if practical.

## References

- Read `references/agentflow-hosted-mode.md` when the work runs inside Agentflow and the problem may involve hosted-runtime assumptions, `runtime.json`, host token, or registration behavior.
- Read `../it-runner-agentd-control/SKILL.md` when the work targets remote Windows program control through `agentd.exe` and `agentctl`.
- Read `references/api-and-debugging.md` when troubleshooting via API.
- Read `references/authoring-checklist.md` when creating or reviewing `.it-runner` tasks.
- Read `references/env-conventions.md` when designing or refactoring `.it-runner` env layouts.
- Read `references/task-reading-and-naming.md` when the problem is understanding task names, task summaries, or cross-project task ambiguity.
- Read `references/log-reading-and-paths.md` when the problem is choosing between `latest` and one concrete run directory, or deciding which log file to read first.
- Read `references/ui-semantics.md` when interpreting task detail statuses, frontend copy behavior, or HTTP-safe copy flows.
- Read `references/task-centric-patterns.md` when choosing between target/meta, lightweight build/dev, app dev+deploy, or control-hub task models.
- Read `references/project-rollout-status.md` when choosing the next repo to migrate or the closest existing repo template.
