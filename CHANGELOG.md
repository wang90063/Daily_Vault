# 变更历史

本文件记录 OrbitOS 系统的版本变更。日常内容变更不记录在此。

## Unreleased

### 调整
- 统一 Claude、Gemini、Codex 的 skill 路径约定，以 `.agents/skills/` 作为唯一源目录
- 更新 Agent 指令与开发文档，明确 `.claude/skills`、`.codex/skills`、`.gemini/skills` 为兼容路径
- `/update-progress` skill 补充写回规则：支持归档中间物、将日记进展关联待办事项、将项目进展关联到对应步骤，并在不确定时直接向用户确认
- 忽略 `99_系统/归档/` 下的归档产物，避免系统开发提交误带中间文件

### 新增
- `/dev-commit` skill，用于按 `DEV.md` 处理系统文件的分支、提交与推送
- 开发规范补充“修改系统文件前先创建本地 `dev/*` 分支”的硬性要求
- `/reflect` skill，用于复盘 AI agent 在一次交互中的失误，输出错误原因、修正点与防错规则
- `/update-progress` skill，用于轻量化整理项目进度更新，只同步现状、重点与阻塞，不自动展开任务

## v0.2.0 — 2026-03-10

### 新增
- `DEV.md` — 开发指南，定义系统文件与内容文件的边界、开发工作流
- `CHANGELOG.md` — 版本变更历史
- `99_系统/模板/Dev_Plan_Template.md` — 开发计划模板
- `CLAUDE.md` 新增「开发流」section — 分支策略、commit 约定、计划驱动流程

## v0.1.0 — 2026-03-03

### 初始版本
- OrbitOS vault 基础结构
- 14 个 Skills 定义
- 5 个模板（Daily_Note, Project, Content, Wiki, Inbox）
- 17 个领域提示词
- Obsidian Base 数据库定义
- Claude Code / Gemini / Codex 多 Agent 配置
