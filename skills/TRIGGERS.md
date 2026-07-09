# Skill Trigger Cheat Sheet

This file helps quickly choose the right it-runner-related skill in `skills/`.

## `what-is-it-runner`

### Use this skill when you say
- "什么是 it-runner？"
- "为什么要用 it-runner？"
- "it-runner 有什么作用？"
- "只用 AI agent 不够吗？"
- "it-runner 和 Agentflow 是什么关系？"
- "什么时候应该把一个流程做成 it-runner 任务？"
- "什么时候不该用 it-runner？"

### Strong trigger words
- 什么是 it-runner
- 为什么要用 it-runner
- Agentflow 和 it-runner 的关系
- AI agent 和 it-runner 的区别
- durable task
- 任务沉淀
- 日志和状态
- 什么时候适合做成任务

---

## `it-runner-workflow`

### Use this skill when you say
- "帮我创建 .it-runner 任务"
- "为什么任务没显示在 UI 里？"
- "帮我检查 it-runner API"
- "envs-next 是什么意思？"
- "怎么通过 .STATE 文件重启任务？"
- "task.version missing 怎么处理？"
- "logsDir 为什么跑到 /logs 了？"
- "帮我修 it-runner 本身的 bug"
- "这个任务名是什么意思？"
- "为什么这里显示可启动但又是已停止？"
- "latest 日志路径和具体目录有什么区别？"
- "现在这是 Agentflow 托管的 it-runner 还是原版模式？"

### Strong trigger words
- .it-runner
- it-runner
- project.yaml
- task.yaml
- env layering
- env.autoDirs
- env.includeSets
- env.required
- envs-next
- STATE 文件重启
- 任务不显示
- API 调试
- 任务发现
- logsDir
- runner bug
- Agentflow 托管
- runtime.json
- host token
- reregister

---

## `it-runner-agentd-control`

### Use this skill when you say
- "帮我通过 agentctl 控制远程 Windows 程序"
- "帮我接 controlapi / agentd.exe"
- "给我写 agent-task.yaml"
- "帮我做 winagent 远程任务"
- "为什么 task apply / restart 成功了但远端程序没起来？"
- "帮我把本地程序打包并发布给远端 Windows agent"

### Strong trigger words
- agentd
- agentd.exe
- controlapi
- agentctl
- winagent
- agent-task.yaml
- remote windows
- task apply
- task restart
- task status
- task logs
- packageName
- workDir
- logDir
- agent ping

---

## `it-runner-convention-upgrade`

### Use this skill when you say
- "帮我把旧 `.it-runner` 升级到新规范"
- "根据 `--check-project-envs` 结果修这个项目"
- "帮我迁移 `.env.local` / `shared.env` 到新命名"
- "把 legacy `.it-runner` env 布局改成编号 env"

### Strong trigger words
- 升级 `.it-runner` 规范
- legacy env layout
- check-project-envs
- migrate env naming
- 000-defaults.env
- 010-local.env
- shared.env -> 000-defaults.env
- .env.local -> 010-local.env

---

## Quick Selection Patterns

### 用户还在问 it-runner 到底是什么
- Start with `what-is-it-runner`

### 我的 `.it-runner` 任务坏了或没显示
- Start with `it-runner-workflow`

### 我要控制远端 Windows agent 程序
- Start with `it-runner-agentd-control`

### 我要升级旧 `.it-runner` env 规范
- Start with `it-runner-convention-upgrade`

## Common Sequences

### 用户还在学习概念
1. `what-is-it-runner`
2. `it-runner-workflow` after the user moves to authoring/debugging

### 任务存在但 UI/API 行为不对
1. `it-runner-workflow`
2. `agentflow-hosted-mode.md` if the work is inside Agentflow or touches `runtime.json` / host lifecycle

### 远端 Windows agent 控制链路有问题
1. `it-runner-agentd-control`
2. `it-runner-workflow` if local `.it-runner` structure is also suspect

### 旧 env 布局迁移
1. `it-runner-convention-upgrade`
2. `it-runner-workflow` for runtime validation

## Template Companion

- For a concrete folder/file starting point, read `skills/TEMPLATES.md`
- For daily `.it-runner` run/stop/log/envs-next commands, read `skills/IT-RUNNER-CHEATSHEET.md`
- For Agentflow-hosted runner boundaries, read `skills/it-runner-workflow/references/agentflow-hosted-mode.md`
