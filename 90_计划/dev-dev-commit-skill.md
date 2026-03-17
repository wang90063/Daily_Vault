---
type: plan
status: active
related-branch: "dev/dev-commit-skill"
created: "2026-03-17"
---
## 目标
新增一个专门处理 OrbitOS 系统文件提交与推送的 skill，并把“开发前先在本地建立 dev 分支”的要求固化进开发规范。

## 影响范围
- `.gitignore`
- `.agents/skills/dev-commit/`
- `.gemini/commands/dev-commit.toml`
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- `DEV.md`
- `CHANGELOG.md`

## 步骤
- [x] Step 1: 创建本地分支 `dev/dev-commit-skill`
- [x] Step 2: 新增 `dev-commit` skill 与 Gemini 命令入口
- [x] Step 3: 更新开发规范与技能注册
- [x] Step 4: 验证改动并等待用户决定是否提交/推送

## 验收标准
- 新增 `/dev-commit` skill，可指导 Agent 按 `DEV.md` 处理系统文件提交与推送
- 文档明确要求开发前先创建本地 `dev/*` 分支
- `AGENTS.md`、`CLAUDE.md`、`GEMINI.md` 已注册该 skill

## 变更记录
- 基于 `docs: 统一 skill 路径说明` 这次提交的经验，补充“误在 main 上开发时的补救分支策略”
