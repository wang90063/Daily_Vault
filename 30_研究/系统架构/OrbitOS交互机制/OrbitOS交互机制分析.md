---
type: reference
created: 2026-03-04
area: "[[SE_CodeBase]]"
tags: [research, architecture, agent, workflow]
status: complete
---

# OrbitOS 与 Claude Code CLI 交互机制分析

## 概述
`my-vault` (OrbitOS) 并不是一个传统意义上由代码硬编码执行逻辑的应用程序，而是一个**”指令系统 + 结构化数据库”**。它通过自然语言和 Markdown 格式的指令集，驱动大型语言模型（如 Claude Code CLI）去执行特定任务。

## 核心交互链路拆解

整个系统的运作可以归纳为以下四个核心链路：

### 1. 全局规则注入 (Global Rule Injection)
- **触发条件**: 当 Claude Code CLI 在当前工作区启动时。
- **机制**: CLI 会自动读取根目录下的 `CLAUDE.md` 或 `AGENTS.md`。这些文件充当了 Agent 的”系统宪法”（System Prompt）。
- **作用**: 规定了最基础的运行逻辑。例如：定义 `00_收件箱` 到 `99_系统` 各个目录的用途，强制规定”项目必须放在 `20_项目`”、”必须使用中文回复”等。无论执行什么具体任务，这些全局规则都会作为底层上下文约束 Agent 的行为。

### 2. 技能激活 (Skill Activation)
- **触发条件**: 用户在对话中输入特定命令，如 `/start-my-day`, `/kickoff`, `/research`。
- **机制**: OrbitOS 在 `.claude/skills/` 目录下存放了多个子目录，每个目录代表一个”技能包”，其中包含一个 `SKILL.md` 文件。当用户触发命令时，对应的 `SKILL.md` 内容会被提取并包裹在 `<command-name>` 和技能提示词标签内注入到当前对话中。
- **作用**: 提供当前任务的**高优先级专门指令**，覆盖或补充全局规则，使通用型 Agent 瞬间转变为”每日规划师”或”研究协调员”。

### 3. 标准化工作流驱动 (Workflow Driven)
- **机制**: 在每个 `SKILL.md` 中，都用自然语言详细定义了 `WORKFLOW`。以 `/start-my-day` 为例，它规定了明确的步骤：
  1. **静默收集上下文**: 指导 Agent 使用 `Read`、`Glob` 等工具读取昨天的日记、活跃项目和收件箱。
  2. **用户交互**: 指导 Agent 使用 `AskUserQuestion` 工具向用户提问（如”今天的主要目标是什么？”）。
  3. **文件操作**: 规定了如何处理用户输入，比如使用 `Write` 创建新的收件箱文件或使用 `Edit` 更新今天的日记。
- **本质**: 这是用自然语言编写的”程序算法”，Agent 是执行这些算法的计算器。

### 4. 模板与数据一致性 (Template Consistency)
- **机制**: 技能脚本在指示 Agent 创建新文件时，通常会要求引用 `99_系统/模板/` 下的文件（如 `Daily_Note.md` 或 `Project_Template.md`）。
- **作用**: 确保由 AI 生成的内容具有高度一致的 Frontmatter (YAML) 和章节布局，从而将非结构化的对话输出转化为结构化的知识库数据。

## 端到端示例：以 `/kickoff` 为例

为了更具体地说明这些机制如何协同工作，我们来看一个用户执行 `/kickoff 00_收件箱/新点子.md` 时的全过程：

1.  **第一阶段: 全局注入 (Global Injection)**
    - Agent 在启动时已经读取了 `CLAUDE.md`。
    - **Agent 的认知**: “我正在 OrbitOS 中工作，我需要确保所有生成的项目都在 `20_项目/` 下，且格式必须符合 C.A.P. 布局。”

2.  **第二阶段: 技能激活 (Skill Activation)**
    - 用户输入 `/kickoff 00_收件箱/新点子.md`。
    - CLI 识别出 `/kickoff` 命令，从 `.claude/skills/kickoff/SKILL.md` 中提取专门指令并加载。
    - **Agent 的状态更新**: 激活”项目经理 (Project Manager)”人格，加载 `/kickoff` 专属工作流。

3.  **第三阶段: 工作流驱动 - 规划 (Planning)**
    - **技能指令**: “第一步，启动规划 Agent，创建计划文件。”
    - **Agent 动作**: 调用 `Read` 工具读取 `00_收件箱/新点子.md`，然后调用 `Write` 工具在 `90_计划/` 目录下生成一个 `Plan_YYYY-MM-DD_Kickoff_新点子.md`。
    - **交互**: 向用户报告：”计划已生成，请确认。”

4.  **第四阶段: 工作流驱动 - 执行 (Execution)**
    - 用户确认计划后，Agent 激活执行者身份。
    - **技能指令**: “第二步，读取计划文件，并在 `20_项目/` 创建正式笔记。”
    - **Agent 动作**: 调用 `Read` 工具获取刚才的计划内容，结合 `99_系统/模板/Project_Template.md` 的格式，调用 `Write` 工具生成 `20_项目/新点子.md`。

5.  **第五阶段: 状态同步与归档 (State Update & Archive)**
    - **技能指令**: “第三步，将原收件箱文件标记为已处理并移动到归档区。”
    - **Agent 动作**: 调用 `Edit` 工具修改 `00_收件箱/新点子.md` 的 frontmatter (`status: processed`)，并调用 `Bash` 工具将其移动到 `99_系统/归档/` 目录。
    - **最终结果**: 整个系统状态保持一致，新项目正式落地。

## 总结
通过以上机制，OrbitOS 巧妙地利用了 LLM 的理解和工具调用能力（Function Calling），无需编写复杂的 Python 或 JS 脚本，仅靠 Markdown 文本就构建了一个具有高度自动化能力的个人管理系统。

---
## 相关阅读
- [[OrbitOS目录结构与核心模块分析]]
- [[Agent_Skill机制]]
