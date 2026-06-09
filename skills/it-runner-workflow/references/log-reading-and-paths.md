# Log Reading And Paths

Use this reference when an AI agent or operator needs to interpret `logDir`, distinguish `latest` from one concrete run directory, or decide which log file to read first.

## Path Model

For one task, there are usually three related path concepts:

1. task log root for one concrete run
2. stable `latest` path
3. one concrete file inside either path

Example:

- concrete run dir: `.it-runner/logs/test-transport-client-online/20260425-044819.839590838`
- stable latest dir: `.it-runner/logs/test-transport-client-online/latest`
- latest main log file: `.it-runner/logs/test-transport-client-online/latest/it-runner.log`

## Which Path To Use

### Use the concrete run directory when:

- you need to know exactly which run produced the current files
- you are correlating one restart or one failure window
- the UI needs to show the active run directory name

### Use the `latest` path when:

- you want a stable path to copy into another tool
- you want operators to paste one reusable path
- the path is being shown in a button, dialog, or quick-copy workflow

Rule:

- show the concrete run directory for context
- prefer copying the `latest` path for reuse

## File Reading Order

When debugging a task, prefer this order:

1. task state from API
2. log file list from API
3. `it-runner.log`
4. task-specific stdout/stderr logs
5. `state.json` if present

Reason:

- `it-runner.log` usually explains orchestration, start/stop, env issues, and runner decisions
- stdout/stderr logs explain the process itself
- `state.json` helps confirm runtime metadata, not full narrative

## Common Files

### `it-runner.log`

Read first when you need:

- start/stop/restart history
- runner-side errors
- keepalive or watch decisions
- port readiness or control-file events

### stdout/stderr logs

Read when:

- the child process started but behaved incorrectly
- the runner itself looks healthy
- application-level errors matter more than orchestration

### `state.json`

Read when:

- you need structured runtime state
- you need to confirm current pids, offsets, or metadata
- the text logs are too noisy for a quick structural check

## UI Semantics For Paths

When documenting or reading the UI, keep these meanings separate:

- file path = `latest/<file>`
- directory path = `latest/`
- current run directory = timestamped or run-specific directory

Do not collapse them into one generic “log path”.

## AI Log Reading Pattern

When a user shares a task screenshot or task name and asks “看日志”:

1. identify the project and task
2. identify whether they need stable path or current run context
3. prefer `latest/it-runner.log` for quick reproduction
4. mention the current concrete run directory if the UI shows it
5. only then dive into stdout/stderr if the runner log is not enough

## AI Fix Verification Pattern

When an AI agent fixes a failing task, the verification must use the next run's
logs, not the old failure logs.

Recommended loop:

1. Before editing, read the current state and logs:
   - task API state, including `status`, `running`, `lastError`, `logDir`,
     `runId`, and `processes`
   - `latest/state.json` when reading files directly
   - `latest/it-runner.log`
   - `latest/*.stderr.log`
   - `latest/*.stdout.log`
2. Explain the likely root cause from the evidence.
3. Make the smallest relevant code, task, or env/config fix.
4. Trigger a new run:
   - use `POST /it-runner/api/tasks/{taskKey}/restart`, or
   - write `RESTART <fresh-token>` to the task's `watch.stateFile`
5. Re-read task state and the files under `latest/`.
6. Report whether the new run proves success or still fails.

Success evidence depends on the task type:

- one-shot build/test task: `status=done` or the target command passed
- service task: service process is running and readiness/probe checks passed
- probe/smoke task: the expected check passed in `it-runner.log` or stdout

Important boundaries:

- `latest/state.json` is a read-only state snapshot. Do not edit it to control a
  task.
- The control entry is `watch.stateFile` or the HTTP task API.
- Use a fresh restart token every time; repeated `RESTART same-token` may be
  ignored as already applied.
- After three failed repair attempts, stop changing files and summarize the
  remaining evidence, attempted fixes, and the next decision needed.

## Common Mistakes

- reading only stdout and missing runner-side failures
- copying a one-off timestamped path when the user really needs `latest`
- saying “logDir” without clarifying whether that means a directory or one file
- treating `latest` as a historical run marker instead of a reusable alias
