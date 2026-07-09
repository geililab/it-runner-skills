# Skills Reading Order

Use this file when you want the shortest human-readable sequence for navigating the `skills/` folder.

## Fastest Reading Order

1. `skills/INDEX.md`
   - Start here if you are new to this folder.

2. `skills/NAVIGATION.md`
   - Read this if you want the shortest route by problem type.

3. `skills/TRIGGERS.md`
   - Read this if you want to know which skill should trigger from a prompt.

4. `skills/TEMPLATES.md`
   - Read this if you want folder/file starting points.

5. `skills/IT-RUNNER-CHEATSHEET.md`
   - Read this if your work involves task execution, logs, APIs, `envs-next`, or `.STATE` files.

6. `skills/what-is-it-runner/SKILL.md`
   - Read this when the user is still asking concept questions such as what `it-runner` is, why it exists, or whether the workflow should become a task at all.

7. `skills/it-runner-workflow/references/agentflow-hosted-mode.md`
   - Read this first when the work is happening inside Agentflow and you might otherwise confuse hosted runner lifecycle with standalone runner behavior.

8. `skills/it-runner-workflow/references/env-conventions.md`
   - Read this if your work involves `.it-runner` env naming, layering, selectors, or secret handling.

9. `skills/it-runner-workflow/references/task-centric-patterns.md`
   - Read this if you need to choose the right task-centered `.it-runner` pattern for a repo.

10. `skills/it-runner-workflow/references/project-rollout-status.md`
   - Read this only when you need historical context on known migrated repos or reference templates.

11. `skills/it-runner-convention-upgrade/SKILL.md`
   - Read this if your work is specifically about migrating an existing project from legacy `.it-runner` env naming to the new numbered convention.

## Then Read The Actual Skill

Choose one of these based on the problem you are solving:

- `skills/what-is-it-runner/SKILL.md`
- `skills/it-runner-workflow/SKILL.md`
- `skills/it-runner-agentd-control/SKILL.md`
- `skills/it-runner-convention-upgrade/SKILL.md`

## Shortest Decision Rule

- Need to explain what `it-runner` is or why it exists -> `what-is-it-runner`
- Need `.it-runner` creation/debugging/API/log help -> `it-runner-workflow`
- Need Agentflow-hosted runner lifecycle boundaries -> `agentflow-hosted-mode.md`
- Need remote Windows agent control -> `it-runner-agentd-control`
- Need legacy `.it-runner` env migration -> `it-runner-convention-upgrade`
