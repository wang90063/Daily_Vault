---
type: plan
status: active
related-branch: "dev/reflect-skill"
created: "2026-03-17"
---
## 目标
新增 `/reflect` skill，用于复盘 AI agent 在一次交互中的失误，输出错误原因、修正点与防错规则，并同步接入 OrbitOS 的技能注册与 Gemini 命令入口。

## 影响范围
- `.agents/skills/reflect/`
- `.gemini/commands/reflect.toml`
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `CHANGELOG.md`

## 步骤
- [x] Step 1: 设计 `/reflect` skill 的定位、输入输出与复盘 pipeline
- [x] Step 2: 新增 `reflect` skill 定义与 Gemini 命令入口
- [x] Step 3: 在 Agent 指令和变更日志中注册 `/reflect`

## 验收标准
- `reflect` skill 可通过描述被正确触发，且工作流聚焦 agent 失误复盘
- `AGENTS.md`、`CLAUDE.md`、`GEMINI.md` 的 Skills 列表已同步注册 `/reflect`
- `.gemini/commands/reflect.toml` 可作为 Gemini 命令入口
- `CHANGELOG.md` 记录本次新增能力

## 变更记录
- 第一版默认基于“对话轨迹 + 用户纠正”做复盘，不强依赖命令日志和文件 diff
- 默认只在对话中输出反思报告，除非用户要求，否则不自动落库
