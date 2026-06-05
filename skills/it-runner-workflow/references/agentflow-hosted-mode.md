# Agentflow Hosted Mode

Use this note when `it-runner` is being used from inside Agentflow rather than as
a standalone runner launched directly by an operator.

## Core Rule

Inside Agentflow, default to this mental model:

- Agentflow hosts `it-runner-agentflow`
- the runner starts with `--mode agentflow`
- Agentflow controls lifecycle, registration, reverse proxy routing, and status
- `.it-runner` project/task knowledge is still valid for task authoring and task
  debugging

Do not collapse these two layers into one:

1. `.it-runner` project/task configuration
2. Agentflow-hosted runner lifecycle and registration

## What Stays The Same

These topics still follow the normal `it-runner-workflow` guidance:

- `.it-runner/project.yaml`
- `.it-runner/tasks/<task>/task.yaml`
- env layering and `envs-next`
- task discovery and task naming
- task logs and task API debugging
- `.STATE` restart/stop behavior when a task defines `watch.stateFile`

## What Changes In Agentflow

These topics belong to the hosted runtime model and should not be inferred from
standalone runner conventions:

- `data/it-runner/runtime.json`
- Agentflow settings-page install / start / restart / stop
- host registration or re-register behavior
- host token and Agentflow-provided auth
- reverse-proxy paths such as `/it-runner/...`
- hosted runtime recovery after Agentflow restart

## Do Not Assume

When working in Agentflow, do not assume any of these unless the code and docs
explicitly prove it:

- the runner should be started through `--runner-config it-runner.yaml`
- `runtime.json` should be edited to look like standalone runner mode
- the right fix is to change runner startup flags instead of fixing `.it-runner`
  task configuration
- a task discovery issue is caused by `project.yaml` alone when the real problem
  is Agentflow-hosted scope / registration behavior

## Practical Decision Rule

Use this split:

- If the problem is "how should this task be authored, discovered, started,
  logged, or debugged?" stay in `it-runner-workflow`.
- If the problem is "how does Agentflow host, register, restart, recover, or
  proxy this runner?" treat it as hosted-mode behavior first and avoid applying
  standalone-runner assumptions.

## Typical Failure Pattern

The most common mistake is:

1. user is working inside Agentflow
2. AI sees `.it-runner` and applies standalone runner assumptions
3. AI rewrites hosted runtime expectations as if the runner were launched by a
   local operator
4. `runtime.json`, startup assumptions, or registration flow become incorrect

Avoid this by explicitly separating task-level `.it-runner` knowledge from
Agentflow-hosted runtime knowledge.
