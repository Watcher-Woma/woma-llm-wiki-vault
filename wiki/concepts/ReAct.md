---
title: "ReAct（推理与行动）"
type: concept
tags: [提示工程, 高级技术, 代理, 推理]
sources:
  - raw/02-papers/Goolge-Prompt-Engineering-whitepaper.pdf
  - raw/01-articles/The Complete Guide to AI Prompt Engineering in 2025-2026.md
last_updated: 2026-04-21
---

## 定义

ReAct（**Re**asoning + **Act**ing）是一种将大型语言模型的推理能力与外部行动能力相结合的提示技术。该范式由 Google Research 提出，旨在解决传统 Chain-of-Thought 仅做推理而不与外部环境交互的局限。

核心思想：**思考（Thought）→ 行动（Action）→ 观察（Observation）** 的循环

## 工作原理

ReAct 通过以下循环结构实现：

```
Thought: 分析当前状态，决定下一步行动
Action: 执行某个动作（查询工具、搜索、计算等）
Observation: 观察行动结果
... 重复直到任务完成
```

### 与 Chain-of-Thought 的区别

| 维度 | Chain-of-Thought | ReAct |
|------|-----------------|-------|
| **能力** | 仅推理 | 推理 + 行动 |
| **外部交互** | 无 | 有（工具调用） |
| **适用场景** | 数学、逻辑推理 | 复杂任务、检索、信息整合 |
| **输出** | 最终答案 | 思考-行动-观察链 |

## 核心组件

### 1. Thought（思考）
```markdown
Thought: 我需要先查找 2024 年全球 GDP 排名前五的国家，
        然后对比它们的增长率。
```

### 2. Action（行动）
```markdown
Action: search
Action Input: "2024 global GDP ranking top 5 countries"
```

### 3. Observation（观察）
```markdown
Observation: [搜索结果显示的 GDP 数据]
```

## 使用场景

### 信息检索与整合
- 跨多个来源整合信息
- 执行多步骤研究任务
- 实时数据查询

### 复杂问题解决
```
问题：分析苹果公司近五年的财务表现

Thought: 需要先获取苹果近五年的财报数据...
Action: search[苹果公司 2020-2024 年报]
Observation: 获取到营收、利润数据
Thought: 接下来需要获取行业对比数据...
Action: search[科技行业平均利润率]
Observation: 获取到行业基准
Thought: 现在可以开始分析了...
```

### 代理系统（Agentic Systems）
ReAct 是现代 AI Agent 的核心技术之一：
- [[Claude]] 的工具使用能力
- OpenAI 的 GPTs 和 Assistants API
- Google 的 Agent Development Kit

## 框架对比

| 框架 | 特点 | 适用场景 |
|------|------|----------|
| **CoT** | 纯推理 | 数学、逻辑问题 |
| **ReAct** | 推理 + 行动 | 检索、工具调用、多步骤任务 |
| **ToT** | 多路径探索 | 需要回溯和备选方案的任务 |

## 实现示例

```python
# ReAct 提示模板
react_prompt = """
你是一个助手，会思考、行动和观察。

按照以下格式回答：
Thought: 你需要做什么
Action: 你要执行的行动（如 search, calculate, read）
Action Input: 行动的具体输入
Observation: 行动的结果
... (重复 Thought/Action/Observation 多次)
Thought: 我现在有足够的信息来回答了
Final Answer: [你的最终答案]
```

## 最佳实践

1. **明确行动空间**：预先定义 AI 可以执行的行动类型
2. **提供工具描述**：清晰说明每个工具的功能和返回值
3. **控制循环次数**：设置最大迭代次数防止无限循环
4. **结构化输出**：使用一致的性格-行动-观察格式

## 关联连接
- [[Chain_of_Thought]] — 纯推理方法（ReAct 的基础）
- [[Tree_of_Thoughts]] — 多路径推理（ReAct 的扩展）
- [[Prompt_Engineering]] — 提示工程总览
- [[Context_Engineering]] — ReAct 需要维护对话上下文
