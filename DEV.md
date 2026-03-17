# OrbitOS 开发指南

本文档面向 OrbitOS 系统本身的开发与维护。日常使用请参考 `CLAUDE.md`。

## 项目架构

OrbitOS 是基于 Obsidian vault 的 AI 驱动知识管理系统。仓库中的文件分为两类：

### 系统文件（开发对象）

对 OrbitOS 功能和行为的定义，修改需走开发流程。

| 路径 | 说明 |
|------|------|
| `.agents/skills/` | Skill 定义单一来源（每个 skill 一个目录，含 SKILL.md） |
| `.claude/` | Claude Code CLI 配置（`skills` 指向 `.agents/skills/`） |
| `.gemini/` | Gemini CLI 配置（settings.json + commands/*.toml，`skills` 指向 `.agents/skills/`） |
| `.codex/` | Codex AI 配置（`skills` 指向 `.agents/skills/`） |
| `99_系统/模板/` | Obsidian 模板文件 |
| `99_系统/提示词/` | 领域提示词 |
| `99_系统/数据库/` | Obsidian Base 数据库定义 |
| `CLAUDE.md` | Claude Code Agent 指令 |
| `AGENTS.md` | 通用 Agent 指令 |
| `GEMINI.md` | Gemini Agent 指令 |
| `DEV.md` | 本开发指南 |
| `CHANGELOG.md` | 变更历史 |
| `.gitignore` | Git 忽略规则 |

### 内容文件（使用产物）

日常使用 OrbitOS 产生的文件，直接在 `main` 分支提交即可。

| 路径 | 说明 |
|------|------|
| `00_收件箱/` | 快速收集的想法和信息 |
| `10_日记/` | 每日日记 |
| `20_项目/` | 项目文档 |
| `30_研究/` | 研究笔记 |
| `40_知识库/` | 原子化知识条目 |
| `50_资源/` | 策展内容（Newsletter、产品发布等） |
| `90_计划/` | 执行计划（含开发计划） |

## 计划驱动开发流程

每次对系统文件的变更都遵循以下流程：

### 1. 制定计划

在 `90_计划/` 创建开发计划文档：
- 文件名：`dev-功能名.md`
- 使用模板：`99_系统/模板/Dev_Plan_Template.md`
- 明确目标、影响范围、步骤和验收标准

### 2. 创建分支

在修改任何系统文件之前，必须先在本地创建 `dev/功能名` 分支，不要直接在 `main` 上开始开发。

```bash
git checkout main
git checkout -b dev/功能名
```

分支命名示例：
- `dev/weekly-review-skill` — 新增 skill
- `dev/template-refactor` — 模板重构
- `dev/fix-daily-note-date` — 修复问题

补救策略：
- 如果已经误在 `main` 上修改或提交了系统文件，不要回滚历史；直接从当前 `HEAD` 补建一个对应的 `dev/功能名` 分支，后续以该分支继续推送和提 PR

### 3. 按步骤执行

- 按照计划中的步骤逐一完成
- 每完成一步，在计划文档中勾选 `- [x]`
- 在变更记录中记录关键决策

### 4. 验证

按计划中的验收标准逐项检查：
- 功能是否按预期工作
- 是否影响现有使用流
- 模板/Skill 是否在 Obsidian 中正常运行

### 5. 合并

```bash
git checkout main
git merge dev/功能名
```

合并后更新 `CHANGELOG.md`，记录本次变更。

### 6. 归档计划

- 将计划文档移至 `90_计划/归档/`
- 修改 frontmatter `status: done`
- 删除开发分支：`git branch -d dev/功能名`

## Commit 约定

| 前缀 | 用途 | 适用场景 |
|------|------|---------|
| `feat:` | 新功能 | 新增 Skill、模板、提示词 |
| `fix:` | 修复 | 修复已有功能的问题 |
| `refactor:` | 重构 | 不改变功能的代码调整 |
| `docs:` | 文档 | 更新 CLAUDE.md、DEV.md 等 |
| `content:` | 内容 | 日常笔记、日记等内容变更 |

**规则：**
- 使用中文描述变更内容
- commit message 简洁明了，一行说清楚
- 示例：`feat: 添加 /weekly-review skill`、`content: 添加 3 月 10 日日记`

## 常见开发任务指南

### 添加新 Skill

1. 在 `.agents/skills/` 下创建目录
2. 编写 `SKILL.md`（参考已有 skill 格式）
3. 如需模板，创建 `TEMPLATE.md`
4. 如需 Gemini 命令入口，在 `.gemini/commands/` 添加对应 `.toml` 配置
5. 不要在 `.claude/skills/`、`.codex/skills/`、`.gemini/skills/` 中复制 skill 内容；它们统一复用 `.agents/skills/`
6. 在 `CLAUDE.md`、`AGENTS.md`、`GEMINI.md` 的 Skills 列表中注册

### 提交开发变更

1. 优先使用 `/dev-commit` skill 处理系统文件的分支、暂存、commit 和 push
2. 只暂存和本次任务相关的文件，不要用 `git add .`
3. 默认把系统文件变更推送到 `origin dev/功能名`，不要直接推 `main`

### 修改模板

1. 修改 `99_系统/模板/` 中的对应文件
2. 在 Obsidian 中验证模板渲染效果
3. 确认 frontmatter 格式正确（`---` 后无空行）

### 修改 Agent 指令

1. 同步修改 `CLAUDE.md` 和 `AGENTS.md`（两者内容应保持一致）
2. 如涉及 Gemini，同步更新 `GEMINI.md`
3. 验证 Agent 行为是否符合预期

### 修改提示词

1. 在 `99_系统/提示词/` 中修改对应文件
2. 测试提示词效果
