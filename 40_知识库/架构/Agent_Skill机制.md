---
type: concept
created: 2026-03-04
area: "[[SE_CodeBase]]"
tags: [architecture, agent, skill]
---

# Agent_Skill机制

Agent Skill (智能体技能) 是 OrbitOS 中用于扩展和模块化 AI 能力的核心机制。它将针对特定场景的**提示词（Prompt）**、**工作流（Workflow）**和**规则（Rules）**打包在一个独立的文件中（通常是 `.agents/skills/<skill-name>/SKILL.md`）。

## 核心特征
1. **按需激活**: 技能不会污染全局上下文，只有在用户输入特定命令（如 `/kickoff`）时才被加载。
2. **多智能体协作**: 复杂的技能（如 `/research` 或 `/kickoff`）通常采用“规划者 (Planner)”与“执行者 (Executor)”双 Agent 架构。规划者负责构建策略结构，执行者带着干净的上下文（Clean Context）仅根据计划生成内容，以此节省 Token 消耗并提高准确性。
3. **工具链整合**: 技能脚本指导 Agent 何时使用搜索（`grep_search`）、读取（`read_file`）或用户交互（`ask_user`）工具。

## 相关概念
- [[OrbitOS交互机制分析]]
