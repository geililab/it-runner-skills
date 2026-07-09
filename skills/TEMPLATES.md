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

## Minimal `task.yaml`

New tasks should include `watch.stateFile` by default so AI agents and
operators can trigger restart/stop through `STATE`. Omit it only for a
deliberate one-shot task that must not expose file-based control.

```yaml
version: "1"
name: example-task
description: Run the example task
tags: [example]
watch:
  stateFile: ${PROJECT_ROOT}/.it-runner/states/example-task.STATE
command:
  - bash
  - ./scripts/example-task.sh
workdir: ${PROJECT_ROOT}
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
- command is non-interactive unless explicitly intended
- workdir points at `${PROJECT_ROOT}` when appropriate
- `watch.stateFile` is present by default unless file-based control is deliberately disabled
- task is visible in API before deeper debugging

## Env Layout Checklist

- shared project defaults live in `.it-runner/envs/000-defaults.env`
- machine-local overrides live in `.it-runner/envs/010-local.env`
- task-local defaults live in `.it-runner/tasks/<task>/envs/000-defaults.env` when needed
- secrets use `SECRET_` prefixes so env inspection can redact them
- legacy `.it-runner/.env` and `.it-runner/.env.local` are not used
