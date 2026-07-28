# It-Runner Templates

This file contains minimal `.it-runner` starting points for projects and tasks.

## Minimal Project Layout

```text
.it-runner/
  project.yaml
  envs/
    000-defaults.env
    010-local.env
  tasks/
    <task-name>/
      task.yaml
  logs/
  states/
```

## Minimal `project.yaml`

```yaml
name: example-project
tasksDir: .it-runner/tasks
logsDir: ${PROJECT_ROOT}/.it-runner/logs
```

Use explicit `envFiles` only for special cases such as repo-external env files:

```yaml
envFiles:
  - /outside/of/repo/secret-overrides.env
```

## Minimal Service `task.yaml`

Use `processes[].cmd` for long-running services such as dev servers, daemons,
watchers, and local app servers. Do not use top-level `command` for this shape:
if the runner does not convert the task into a managed process, the run can
finish immediately with `done (no processes)`.

New service tasks should include `watch.stateFile` by default so AI agents and
operators can trigger restart/stop through `STATE`.

```yaml
version: "1"
name: dev-server
description: Run the local dev server
tags: [dev]
watch:
  stateFile: ${PROJECT_ROOT}/.it-runner/states/dev-server.STATE
processes:
  - name: dev-server
    cmd: |-
      bash -lc 'cd "${PROJECT_ROOT}/presentation" && exec npm run dev -- --host 0.0.0.0 --port 5174 --strictPort --force'
```

Use an explicit `cd "${PROJECT_ROOT}/..." && exec ...` in `cmd` unless the
runner version's task-level workdir semantics have already been verified.

## Minimal One-Shot `task.yaml`

Use the runner's supported one-shot shape only for checks, builds, scripts, and
other commands that are expected to exit. Omit `watch.stateFile` only when the
task intentionally should not expose file-based control.

```yaml
version: "1"
name: example-check
description: Run the example check
tags: [check]
watch:
  stateFile: ${PROJECT_ROOT}/.it-runner/states/example-check.STATE
command:
  - bash
  - -lc
  - cd "${PROJECT_ROOT}" && ./scripts/example-check.sh
```

## Recommended `stateFile` Control

If a task defines `watch.stateFile`, use these control commands:

- restart: `echo "RESTART $(date +%s)" > <STATE_FILE>`
- stop: `echo "STOP $(date +%s)" > <STATE_FILE>`

Use a fresh timestamp or token each time. Do not write restart commands to
`latest/state.json`; that file is a read-only run snapshot, not the control
entry.

## Task Authoring Checklist

- task lives at `<task-dir>/task.yaml`
- `version: "1"` is present
- name is operator-friendly
- long-running services use `processes[].cmd`, not top-level `command`
- one-shot commands are non-interactive unless explicitly intended
- service commands include an explicit `cd "${PROJECT_ROOT}/..." && exec ...` when task-level workdir behavior has not been proven
- `watch.stateFile` is present by default unless file-based control is deliberately disabled
- task is visible in API before deeper debugging
- after a service start, `it-runner.log` does not say `done (no processes)`
- service readiness is verified through task state (`running: true`), a passing probe, or an external signal such as a listening port

## Env Layout Checklist

- shared project defaults live in `.it-runner/envs/000-defaults.env`
- machine-local overrides live in `.it-runner/envs/010-local.env`
- task-local defaults live in `.it-runner/tasks/<task>/envs/000-defaults.env` when needed
- secrets use `SECRET_` prefixes so env inspection can redact them
- legacy `.it-runner/.env` and `.it-runner/.env.local` are not used
