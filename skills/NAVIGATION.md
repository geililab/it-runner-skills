# Skills Navigation Map

## Fast Path

```text
用户先问什么是 it-runner / 为什么要用它
  -> what-is-it-runner

.it-runner / API / 日志 / envs-next / .STATE 有问题
  -> it-runner-workflow
  -> agentflow-hosted-mode.md (if the runner is hosted by Agentflow)

agentd.exe / controlapi / agentctl / 远端 Windows 程序控制
  -> it-runner-agentd-control

旧 `.it-runner` env 命名要迁移到新编号规范
  -> it-runner-convention-upgrade
```

## Companion Files

```text
只想看触发词
  -> TRIGGERS.md

只想看命令和排障手法
  -> IT-RUNNER-CHEATSHEET.md

只想照目录模板搭起来
  -> TEMPLATES.md
```

## Recommended Reading Order

```text
INDEX.md
  -> TRIGGERS.md
  -> TEMPLATES.md
  -> 具体 SKILL.md
```

## Suggested Paths

### User Education

```text
what-is-it-runner
  -> it-runner-workflow (after the user moves from concept to authoring/debugging)
```

### Broken Tasks

```text
it-runner-workflow
  -> agentflow-hosted-mode.md (if running inside Agentflow)
  -> IT-RUNNER-CHEATSHEET.md
```

### Remote Windows Control

```text
it-runner-agentd-control
  -> it-runner-workflow (if local task/env structure also needs work)
  -> IT-RUNNER-CHEATSHEET.md
```

### Legacy Env Migration

```text
it-runner-convention-upgrade
  -> it-runner-workflow
  -> agentflow-hosted-mode.md (if migration validation happens inside Agentflow)
  -> IT-RUNNER-CHEATSHEET.md
```
