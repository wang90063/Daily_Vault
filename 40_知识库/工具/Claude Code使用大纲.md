---
type: concept
created: 2026-03-25
area: "[[SE_CodeBase]]"
tags: [claude-code, anthropic, 工具, 配置]
---
# Claude Code使用大纲
> 注：本大纲按 2026-03-25 可查到的 Claude Code 官方公开文档与 PackyAPI 文档整理。你提到的 `/plan`、`/fork`、`/btw` 存在版本差异、插件差异或公开文档未完全同步的问题，最终应以你本机 `claude` 会话中的 `/help` 和实际已安装插件为准。

## 一、Claude Code 是什么
- Claude Code 是 Anthropic 提供的终端型 AI 编程助手，适合在本地仓库里做阅读、修改、调试、重构、提交等工作。
- 它和网页版 Claude 的区别在于：Claude Code 直接运行在终端中，天然面向代码库、命令行和工程工作流。
- 这篇文档建议从“安装能跑通”开始，再进入“配置 API”“搭项目记忆”“装技能和命令”。

## 二、安装
### 1. 官方推荐安装方式
- 原生安装，官方当前更推荐：
  - macOS / Linux / WSL：`curl -fsSL https://claude.ai/install.sh | bash`
  - Windows PowerShell：`irm https://claude.ai/install.ps1 | iex`
- Homebrew 安装：
  - `brew install --cask claude-code`
- WinGet 安装：
  - `winget install Anthropic.ClaudeCode`

### 2. npm 安装
- 兼容方式：
  - `npm install -g @anthropic-ai/claude-code`
- 备注：
  - 按 2026-03-25 官方文档，npm 安装已被标记为 `deprecated`，更建议使用原生安装。
  - 如果只是为了先跑起来，这一节仍然可以保留，方便读者理解旧教程。

### 3. 安装后验证
- 查看版本：
  - `claude --version`
- 做一次健康检查：
  - `claude doctor`
- 启动交互式会话：
  - `claude`

## 三、API 配置（PackyAPI）
### 1. 配置思路
- 官方 `settings.json` 支持通过 `env` 注入环境变量。
- 官方 LLM gateway 文档支持通过 `ANTHROPIC_BASE_URL` 把 Claude Code 指向第三方 Anthropic 兼容网关。
- PackyAPI 可以按这一类网关方式接入。

### 2. 推荐放置位置
- 用户级全局配置：
  - `~/.claude/settings.json`
- 项目级共享配置：
  - `.claude/settings.json`
- 项目级本地配置：
  - `.claude/settings.local.json`

### 3. PackyAPI 示例配置
```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://www.packyapi.com",
    "ANTHROPIC_AUTH_TOKEN": "你的_PackyAPI_Token",
    "CLAUDE_CODE_ATTRIBUTION_HEADER": "0"
  }
}
```
- 可拆解成三层理解：
  - `ANTHROPIC_BASE_URL`：把请求发到 PackyAPI。
  - `ANTHROPIC_AUTH_TOKEN`：把令牌作为 `Authorization` 头发送。
  - `CLAUDE_CODE_ATTRIBUTION_HEADER`：这是 PackyAPI 文档给出的接入项，属于 Packy 侧补充配置，不是我在官方环境变量页里看到的核心通用项。

### 4. 可选的更高级配置
- 如果以后要接入动态令牌，可以引出：
  - `apiKeyHelper`
  - `CLAUDE_CODE_API_KEY_HELPER_TTL_MS`
- 这一节可以只做提纲，不必展开脚本实现。

### 5. 配置完成后的验证
- 重启终端。
- 运行：
  - `claude`
- 进入会话后检查：
  - `/status`
- 如果仍提示登录或连不上：
  - 先检查 `ANTHROPIC_BASE_URL`
  - 再检查 `ANTHROPIC_AUTH_TOKEN`
  - 最后确认是否真的写进了当前生效的 `settings.json`

## 四、Claude Code 的目录与配置结构
### 1. 用户级结构
- `~/.claude/settings.json`
- `~/.claude/CLAUDE.md`
- `~/.claude/skills/`
- `~/.claude/commands/`
- `~/.claude/agents/`

### 2. 项目级结构
- `./CLAUDE.md` 或 `./.claude/CLAUDE.md`
- `./.claude/settings.json`
- `./.claude/settings.local.json`
- `./.claude/skills/`
- `./.claude/rules/`
- `./.claude/commands/`
- `./.claude/agents/`

### 3. 插件级结构
- `.claude-plugin/plugin.json`
- `commands/`
- `skills/`
- `agents/`
- `hooks/`
- `.mcp.json`

### 4. 这些东西分别负责什么
- `CLAUDE.md`：
  - 负责长期指令、项目约束、工作流偏好、架构说明。
- `settings.json`：
  - 负责客户端行为、权限、插件、环境变量、MCP 等配置。
- `skills/`：
  - 负责复杂能力的模块化封装，通常是 `SKILL.md + 参考文件 + scripts + templates`。
- `commands/`：
  - 负责显式调用的命令入口。
- `plugins`：
  - 负责把命令、skills、agents、hooks、MCP 一起打包分发。

### 5. 一个值得单独讲清的变化
- 按当前官方文档，自定义 commands 正在向 skills 体系收敛。
- 旧的 `.claude/commands/*.md` 依然兼容，但新文档已经明确把“自定义命令”和“skills”放在同一套扩展思路里讲。

## 五、从 `CLAUDE.md` 开始
### 1. `/init` 是起点
- `/init` 会为项目生成起步版 `CLAUDE.md`。
- 如果已经存在 `CLAUDE.md`，它更倾向于提出改进建议，而不是直接覆盖。
- 新版初始化流程还能一起帮助搭：
  - `CLAUDE.md`
  - `skills`
  - `hooks`

### 2. `CLAUDE.md` 里建议写什么
- 项目的构建命令与测试命令。
- 核心目录结构和模块职责。
- 命名规范、提交规范、Review 关注点。
- 常见开发流程，例如“改完要先跑什么命令”。

### 3. `CLAUDE.md` 的写法原则
- 尽量短。
- 尽量具体。
- 尽量可验证。
- 超过一定体量后，拆到 `.claude/rules/`。
- 如果已有其他文档，可用 `@path/to/file` 导入。

## 六、Skills 的安装与使用
### 1. Skills 是什么
- Skill 是 Claude Code 的能力包。
- 它既可以被 Claude 在上下文合适时自动发现，也可以被你显式通过 `/skill-name` 调用。
- 相比一条简单命令，Skill 更适合承载多文件说明、脚本、模板和复杂工作流。

### 2. Skills 的安装位置
- 用户级：
  - `~/.claude/skills/<skill-name>/SKILL.md`
- 项目级：
  - `.claude/skills/<skill-name>/SKILL.md`
- 插件级：
  - `plugin-root/skills/<skill-name>/SKILL.md`

### 3. 一个最小可用的 Skill 结构
```text
my-skill/
├── SKILL.md
├── reference.md
├── scripts/
└── templates/
```

### 4. `SKILL.md` 的关键字段
- `name`
- `description`
- `allowed-tools`
- `disable-model-invocation`
- `context: fork`
- `agent`

### 5. Skills 与命令的关系
- 旧教程常把 `.claude/commands/` 单独讲成“自定义 slash commands”。
- 新文档更强调：很多这类能力现在都可以直接收敛成 Skill。
- 所以这一篇建议单独加一节“命令体系演进”，避免读者继续把 commands 和 skills 看成完全分离的两套东西。

### 6. 如何分发 Skills
- 最轻量：
  - 直接把 `.claude/skills/` 提交进仓库。
- 更规范：
  - 通过插件和 marketplace 分发。

## 七、命令体系总览
### 1. CLI 命令
- `claude`
- `claude "task"`
- `claude -p "query"`
- `claude -c`
- `claude -r "<session-id>"`
- `claude update`
- `claude mcp`

### 2. 官方内置 slash commands
- `/init`
- `/help`
- `/config`
- `/status`
- `/memory`
- `/rewind`
- `/model`
- `/permissions`
- `/review`
- `/mcp`
- `/doctor`
- `/clear`

### 3. 你特别关心的几个命令
- `/init`
  - 初始化项目级 `CLAUDE.md`，是配置项目记忆和规范的入口。
- `/plan`
  - 建议在文档里写成“Plan Mode / 规划模式”而不是只当作一个普通命令。
  - 公开官方文档里，Plan Mode 主要是作为权限模式来描述，常见进入方式是 `Shift+Tab` 切换模式。
  - 如果你本机版本里真的直接提供 `/plan`，那一节可以补一句“以 `/help` 实际显示为准”。
- `/rewind`
  - 依赖 checkpointing，用来回退对话和/或 Claude 做出的文件编辑。
- `/fork`
  - 当前官方公开文档里，我更明确看到的是 Skill frontmatter 里的 `context: fork`，表示把任务放进隔离上下文或子代理里执行。
  - 至于 `/fork` 是否作为稳定内置命令出现，建议在大纲里标成“版本相关 / 待按本机帮助校对”。
- `/btw`
  - 我没有在当前公开官方文档中看到稳定命令说明。
  - 更稳妥的写法是：先标为“实验能力、隐藏命令或插件命令，需以本机 `/help` 和来源插件说明为准”。
- `/skill-creator`
  - 这不是默认内置命令。
  - 它来自 Anthropic Verified 的 `Skill Creator` 插件。
  - 安装插件后才会出现，用来创建、评测、改进和基准测试 skills。

### 4. 自定义命令与插件命令
- 项目命令：
  - `.claude/commands/*.md`
- 用户命令：
  - `~/.claude/commands/*.md`
- 插件命令：
  - 通过 `/plugin` 安装后出现在 `/help` 中。

## 八、`/skill-creator` 这一节可以怎么写
### 1. 它是什么
- Anthropic 官方验证插件。
- 目标是帮助你创建、评估、改进和 benchmark 技能。

### 2. 怎么安装
- 通过 `/plugin` 打开插件管理界面。
- 搜索并安装 `Skill Creator`。
- 安装后重启 Claude Code。

### 3. 怎么使用
- 直接输入：
  - `/skill-creator`
- 再根据模式进入：
  - Create
  - Eval
  - Improve
  - Benchmark

## 九、推荐学习顺序
- 第一步：安装 Claude Code。
- 第二步：先把 PackyAPI 跑通。
- 第三步：用 `/init` 生成项目级 `CLAUDE.md`。
- 第四步：把个人偏好补到 `~/.claude/CLAUDE.md`。
- 第五步：再做自己的 `skills/`。
- 第六步：最后再补插件市场、团队分发和高级工作流。

## 十、后续可扩展章节
- Claude Code 与 Git 工作流。
- Claude Code 与 MCP。
- Claude Code 与插件市场。
- `.claude/rules/` 的拆分策略。
- 团队共享 `CLAUDE.md`、`skills`、`plugins` 的方式。
- 常见报错与排查清单。

## 相关概念
- [[Agent_Skill机制]]
- [[网络与SSL配置]]

## 参考资料
- Claude Code 安装与高级配置：https://code.claude.com/docs/en/getting-started
- Claude Code 配置与 `settings.json`：https://code.claude.com/docs/en/settings
- Claude Code 记忆与 `CLAUDE.md`：https://code.claude.com/docs/en/memory
- Claude Code Skills：https://code.claude.com/docs/en/slash-commands
- Claude Code Checkpointing / `/rewind`：https://code.claude.com/docs/en/checkpointing
- Claude Code Plugins：https://code.claude.com/docs/en/plugins
- PackyAPI 的 Claude Code 配置：https://docs.packyapi.com/docs/cli/claude
