---
title: "CRAFT Framework"
type: concept
tags: [提示工程, 框架, 结构化提示]
sources:
  - raw/09-archive/The Complete Prompt Engineering Guide (2025).md
last_updated: 2026-04-21
---

## 定义

CRAFT 框架是一个结构化的提示设计方法论，包含五个核心要素：**C**ontext、**R**ole、**A**ction、**F**ormat、**T**one。该框架由 BrilliantPrompts 提出，旨在使提示工程变得系统化和可重复。

## 框架详解

| 要素 | 全称 | 说明 | 示例 |
|------|------|------|------|
| **C** | Context | 设置场景，回答"我是谁？情况是什么？" | "我是某 SaaS 公司的营销经理，负责月活 10 万的产品" |
| **R** | Role | 指定 AI 应扮演的角色，激活相关专业知识 | "你是一位有 10 年经验的内容营销专家" |
| **A** | Action | 明确要执行的具体任务 | "撰写一篇 1,200 字的博客文章" |
| **F** | Format | 指定输出结构 | "包含标题、引言、3 个要点、结论" |
| **T** | Tone | 定义风格和语气 | "专业但平易近人，避免行话" |

## 框架对比

| 框架 | 适用场景 | 复杂度 |
|------|----------|--------|
| **5C Framework** | 极简高效提示，token 受限场景 | 低 |
| **CRAFT** | 需要角色扮演的结构化任务 | 中 |
| **CO-STAR** | 内容创作与营销文案 | 中 |
| **APE** | 行动导向的任务执行 | 低 |
| **RISEN** | 复杂多步骤任务 | 高 |

## 使用示例

**输入（CRAFT 结构）：**
```
C: 我是一家 20 人创业公司的创始人，即将推出项目管理工具
R: 你是一位经验丰富的 SaaS 增长营销专家，擅长冷启动
A: 设计一套 3 个月的获客策略，包含渠道选择和预算分配
F: 分三个阶段呈现，每阶段包含目标、策略、执行步骤
T: 直接务实，有数据支撑
```

**输出质量提升：**
- 比无结构提示减少 60% 的迭代次数
- 角色设定激活相关专业知识
- 格式要求避免冗余信息

## CRAFT vs 5C

CRAFT 与 5C Framework 有相似之处，但有关键区别：

| 维度 | 5C | CRAFT |
|------|-----|-------|
| **起源** | 学术研究 | 实践优化 |
| **核心** | Character/Cause/Constraint/Contingency/Calibration | Context/Role/Action/Format/Tone |
| **Token 效率** | ~54 tokens 平均 | ~80-120 tokens |
| **关注点** | 契约式极简提示 | 结构化任务执行 |

> "5C 更适合需要极简高效的场景；CRAFT 更适合需要明确结构和角色设定的复杂任务。" — 实践总结

## 关联连接
- [[Prompt_Engineering]] — 提示工程总览
- [[5C_Framework]] — 5C 提示契约框架
- [[摘要-complete-prompt-engineering-guide-2025]] — 来源摘要
- [[Few_Shot_Prompting]] — 少样本提示（可结合使用）
