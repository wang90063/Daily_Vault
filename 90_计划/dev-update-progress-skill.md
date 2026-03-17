---
type: plan
status: active
related-branch: "dev/update-progress-skill"
created: "2026-03-17"
---
## 目标
新增 `/update-progress` skill，用于轻量化整理一次项目进度更新，只输出当前状态、已完成、当前重点与阻塞，不自动扩展成规划、执行或额外交付。

## 影响范围
- `.agents/skills/update-progress/`
- `.gemini/commands/update-progress.toml`
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `CHANGELOG.md`
- `.gitignore`

## 步骤
- [x] Step 1: 明确 skill 定位、边界与最小输出结构
- [x] Step 2: 新增 `update-progress` skill 与 Gemini 命令入口
- [x] Step 3: 在 Agent 指令与变更日志中注册新 skill

## 验收标准
- `update-progress` 的描述能清楚触发“轻量化进度同步”场景
- skill 明确限制默认行为，不自动升级为规划、执行或落库
- `AGENTS.md`、`CLAUDE.md`、`GEMINI.md` 已同步注册 `/update-progress`
- `.gemini/commands/update-progress.toml` 可作为 Gemini 命令入口
- `CHANGELOG.md` 已记录本次新增能力

## 变更记录
- 默认将“更新一下进度”“先记一下当前进展”视为状态同步而不是执行请求
- 默认只在对话中整理进度，不自动创建笔记或待办
- 用户明确要求写回时，支持把中间物归档到 `99_系统/归档/收件箱/YYYY/MM/`，并在进展中建立链接
- 用户明确要求写回时，要求把日记中的 progress 关联到对应待办，把项目中的 progress 关联到对应步骤；无法判断时直接问用户
- 忽略 `99_系统/归档/` 下的归档产物，避免系统开发提交流程误带中间文件
