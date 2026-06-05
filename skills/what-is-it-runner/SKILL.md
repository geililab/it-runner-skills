---
name: what-is-it-runner
description: Use when a user asks what it-runner is, why it exists, how it relates to Agentflow or AI agents, when it should be used, or whether a task belongs in it-runner at all.
---

# What Is It-Runner

## Overview

`it-runner` is a task runner for turning repeatable project operations into named
tasks with explicit startup behavior, logs, state, and control surfaces.

It is useful when a team no longer wants important project operations to live only
in tribal knowledge, ad-hoc shell history, or one-off AI conversations.

## When to Use

Use this skill when the user is asking concept questions such as:

- what `it-runner` is
- why a team would adopt `it-runner`
- how `it-runner` differs from a plain AI agent session
- how `it-runner` fits into Agentflow
- when a workflow should become an `it-runner` task
- when not to use `it-runner`

Do not use this skill as the main guide for task authoring, task discovery, API
debugging, or runtime bug fixing. For those, switch to
`../it-runner-workflow/SKILL.md`.

## Core Explanation

Explain `it-runner` in this order:

1. It turns repeatable project operations into named tasks.
2. Those tasks have logs, state, start/stop/restart behavior, and a stable place
   for humans and AI to find them.
3. It complements AI agents rather than replacing them.
4. In Agentflow, it provides the task/runtime layer while AI provides the analysis,
   code changes, and follow-up handling.

## Why Not Only Use AI Agents

The clearest explanation is:

- AI agents are good at understanding a problem, editing code, and proposing a fix.
- `it-runner` is good at making a repeatable operation explicit and operable.

Without `it-runner`, a team often ends up with:

- important commands hidden in chat history
- inconsistent startup steps between people
- no stable task logs
- no shared task lifecycle control
- repeated re-explanation of how to run the same thing

With `it-runner`, the operation becomes a durable task instead of a one-time chat
instruction.

## Good Use Cases

Recommend `it-runner` for:

- local dev servers
- backend services
- test watchers
- build tasks that operators repeat often
- packaging and deploy-prep tasks
- remote-control wrapper tasks
- tasks that humans and AI should both run the same way

## Poor Use Cases

Do not recommend `it-runner` for:

- one-off shell commands
- throwaway investigation steps
- tiny actions that will never be reused
- cases where a durable task surface would add more maintenance than value

## Agentflow Relationship

When the user is in Agentflow, explain the relationship like this:

- Agentflow is the workspace and AI collaboration surface.
- `it-runner` is the task execution and task observability surface.
- AI can create, fix, and operate tasks.
- `it-runner` gives those tasks stable names, logs, and lifecycle controls.

If the user asks about hosted runtime behavior, registration, or `runtime.json`,
also read `../it-runner-workflow/references/agentflow-hosted-mode.md`.

## Common Misunderstandings

Correct these misunderstandings directly:

- "`it-runner` replaces AI" -> No. It gives AI and humans a durable task surface.
- "`it-runner` is only for deployment" -> No. It is also for local dev, testing,
  builds, diagnostics, and control flows.
- "`it-runner` is only a YAML format" -> No. The value is not just configuration;
  it is discoverability, repeatability, logs, and lifecycle control.
- "If AI can type commands, we do not need `it-runner`" -> AI can run commands,
  but `it-runner` makes recurring operational knowledge persistent and shareable.

## Next Step Routing

Route the user based on intent:

- wants to create or debug tasks -> `../it-runner-workflow/SKILL.md`
- wants remote Windows control through `agentd.exe` / `agentctl` ->
  `../it-runner-agentd-control/SKILL.md`
- wants to migrate old `.it-runner` env layout ->
  `../it-runner-convention-upgrade/SKILL.md`
