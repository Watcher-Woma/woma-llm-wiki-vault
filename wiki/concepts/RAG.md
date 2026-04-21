---
title: "RAG（检索增强生成）"
type: concept
tags: [提示工程, 高级技术, 检索, 知识库]
sources:
  - raw/02-papers/Goolge-Prompt-Engineering-whitepaper.pdf
  - raw/01-articles/The Complete Guide to AI Prompt Engineering in 2025-2026.md
last_updated: 2026-04-21
---

## 定义

RAG（**R**etrieval-**A**ugmented **G**eneration，检索增强生成）是一种将信息检索系统与大型语言模型相结合的技术架构。RAG 通过从外部知识库中检索相关文档，为 LLM 提供最新、最准确的上下文信息，从而解决模型"知识过时"或"知识不足"的问题。

> "RAG = 检索（Retrieval）+ 生成（Generation）" — Lewis et al., 2020

## 核心架构

```
用户问题 → 检索器 → 相关文档 → 与用户问题拼接 → LLM 生成 → 最终回答
                                  ↑
                           知识库/向量数据库
```

### 三阶段流程

1. **索引阶段（Indexing）**
   - 文档分块（Chunking）
   - 向量化（Embedding）
   - 存入向量数据库

2. **检索阶段（Retrieval）**
   - 将用户问题向量化
   - 相似度搜索
   - 返回 Top-K 相关文档

3. **生成阶段（Generation）**
   - 将检索结果注入提示词
   - LLM 基于上下文生成回答
   - 引用来源标注

## 关键组件

### 1. 文档分块（Chunking）
| 方法 | 块大小 | 适用场景 |
|------|--------|----------|
| 固定大小 | 512/1024 tokens | 通用场景 |
| 语义分块 | 按段落/主题 | 长文档、书籍 |
| 递归分块 | 多层级 | 结构化文档 |

### 2. 向量化模型（Embedding）
- **通用型**：OpenAI text-embedding-3-large
- **中文优化**：BGE、text2vec、m3e
- **领域专用**：SciBERT、CodeBERT

### 3. 向量数据库
| 数据库 | 特点 |
|--------|------|
| **Pinecone** | 云原生、高可用 |
| **Weaviate** | 开源、混合搜索 |
| **Chroma** | 轻量级、本地部署 |
| **Milvus** | 大规模向量检索 |

## 与 Fine-tuning 的对比

| 维度 | RAG | Fine-tuning |
|------|-----|------------|
| **知识更新** | 实时（更新索引即可） | 需要重新训练 |
| **成本** | 低（无需训练） | 高（GPU + 时间） |
| **可解释性** | 高（可溯源） | 低（隐含在权重中） |
| **幻觉风险** | 较低（基于检索内容） | 取决于训练质量 |
| **适用场景** | 知识库问答、实时信息 | 风格适配、复杂推理 |

## 提示工程中的 RAG

在 RAG 系统中，提示词设计至关重要：

### 基础 RAG 提示
```markdown
基于以下上下文回答用户问题。如果上下文中没有相关信息，
请明确说明"我无法从提供的文档中找到答案"。

上下文：
{retrieved_documents}

问题：{user_question}

回答：
```

### 高级技巧

1. **上下文压缩**
   - 使用 LLMLingua 等技术压缩检索结果
   - 减少 token 消耗同时保留关键信息

2. **混合检索**
   - 结合稠密检索（Embedding）和稀疏检索（BM25）
   - 提升召回率和准确性

3. **重排序（Reranking）**
   - 使用 Cross-encoder 对检索结果重排序
   - 提升 top-K 结果的相关性

## 应用场景

| 场景 | 说明 |
|------|------|
| **企业知识库** | 客服、内部文档检索 |
| **学术研究** | 论文摘要、文献综述 |
| **代码助手** | 代码检索、API 文档问答 |
| **医疗健康** | 医学文献、临床指南 |
| **法律咨询** | 法规检索、案例分析 |

## 最佳实践

1. **分块策略**：根据内容类型选择合适的分块大小
2. **元数据保留**：保留文档来源、时间、标题等信息
3. **查询优化**：使用查询扩展、意图识别提升检索质量
4. **评估迭代**：持续评估 precision/recall，优化检索 pipeline

## 关联连接
- [[Context_Engineering]] — RAG 是 Context Engineering 的核心技术
- [[Prompt_Engineering]] — RAG 系统中的提示词设计
- [[Chain_of_Thought]] — 可与 CoT 结合提升推理能力
