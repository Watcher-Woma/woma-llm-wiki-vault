---
title: "Tree of Thoughts（思维树）"
type: concept
tags: [提示工程, 高级技术, 推理, 多路径]
sources:
  - raw/02-papers/Goolge-Prompt-Engineering-whitepaper.pdf
  - raw/01-articles/The Complete Guide to AI Prompt Engineering in 2025-2026.md
last_updated: 2026-04-21
---

## 定义

Tree of Thoughts（ToT，思维树）是一种结构化的推理框架，通过同时探索多条可能的解决路径来增强大语言模型的推理能力。该框架由 Yao et al. 于 2023 年提出，是 Chain-of-Thought 的扩展版本。

核心思想：将问题解决过程建模为树结构，每个节点代表一个"思考状态"，分支代表不同的推理选择。

## 与 Chain-of-Thought 的对比

| 维度 | Chain-of-Thought | Tree of Thoughts |
|------|-----------------|------------------|
| **结构** | 线性链 | 树形/网络结构 |
| **路径** | 单路径 | 多路径并行 |
| **回溯能力** | 有限 | 完整 |
| **适用问题** | 单解问题 | 多解/需要探索的问题 |
| **计算成本** | 低 | 中-高 |

## 核心机制

### 1. 分解问题
```
问题：设计一款针对 Z 世代的社交 App
       ↓
分解为多个维度：
- 核心功能
- 差异化特性
- 变现模式
- 增长策略
```

### 2. 扩展节点
每个思考节点可以生成多个子思考：

```
"核心功能" 节点
├── 即时通讯 + 兴趣社群
├── 短视频 + 直播互动
├── AI 匹配 + 线下活动
└── 匿名社交 + 情感树洞
```

### 3. 评估与剪枝
- 对每个路径进行评估
- 剪除明显不佳的路径
- 保留有潜力的路径继续探索

### 4. 路径回溯
当某条路径遇到死胡同时，可以回溯到上一个节点选择其他分支。

## 使用场景

### 1. 创意生成
```
任务：为新能源品牌起 10 个中文名

Thought Branch A：强调"清洁"
- 清新能源、清洁派、绿境...

Thought Branch B：强调"未来"
- 未来能源、先驱、领航...

Thought Branch C：强调"动力"
- 动能无限、动力源、澎湃...
```

### 2. 复杂决策
```
任务：选择职业方向

评估维度：
- 薪资水平
- 发展空间
- 个人兴趣
- 工作生活平衡

每个维度展开多个选项，进行系统性评估
```

### 3. 代码调试
```
问题：程序报错 "segmentation fault"

分支探索：
- 内存泄漏 → 检查 malloc/free
- 数组越界 → 检查索引边界
- 空指针引用 → 检查指针初始化
```

### 4. 战略规划
```
任务：制定季度 OKR

展开多个目标路径：
- 用户增长路径
- 产品优化路径
- 营收提升路径
```

## 实现方法

### 方法一：手工构造（Prompt-based）
```markdown
你是一个战略规划专家。请用思维树方法分析以下问题：

问题：{problem}

请按以下步骤进行：
1. 将问题分解为 3-5 个核心维度
2. 每个维度列出 2-3 个可能的方案
3. 评估每个方案的优劣势
4. 推荐最优组合
```

### 方法二：程序化实现
```python
class TreeOfThoughts:
    def __init__(self, model):
        self.model = model
        self.tree = {}
    
    def expand(self, node, n_branches=3):
        """展开节点，生成多个子思考"""
        prompt = f"针对'{node}'，提出{n_branches}个不同的解决方向"
        branches = self.model.generate(prompt)
        return branches
    
    def evaluate(self, node):
        """评估节点质量"""
        prompt = f"评估'{node}'的可行性和潜在价值（1-10分）"
        return self.model.generate(prompt)
    
    def prune(self, nodes):
        """剪枝，保留 top-K"""
        scores = [self.evaluate(n) for n in nodes]
        return sorted(zip(nodes, scores), key=lambda x: x[1])[:3]
```

## 提示词模板

```markdown
## 角色
你是一个擅长系统性思考的策略顾问，擅长用思维树方法解决复杂问题。

## 任务
{problem}

## 方法
请使用 Tree of Thoughts 方法：

1. **问题分解**：将问题拆解为 3-5 个核心维度
2. **路径扩展**：每个维度展开 2-3 个可能的方案
3. **评估打分**：每个方案给出 1-10 分的质量评估
4. **路径回溯**：如果某条路径不通，标记并切换
5. **综合结论**：整合最优路径给出最终建议

## 输出格式
- 树形结构展示思考过程
- 每个节点包含：思考内容、评估分数、下一步选项
- 最终结论包含推荐理由
```

## 最佳实践

1. **适度分叉**：分支过多导致计算成本上升，建议 3-5 个分支
2. **深度限制**：设置最大深度防止无限递归
3. **剪枝策略**：及时剪除明显无效的分支
4. **结合评估**：使用外部评估器或 LLM 自评

## 与其他技术的关系

- **ToT vs CoT**：ToT 是 CoT 的多路径扩展
- **ToT vs ReAct**：ReAct 强调行动，ToT 强调推理探索
- **ToT vs Self-consistency**：两者都使用多路径，但 ToT 是显式树结构

## 关联连接
- [[Chain_of_Thought]] — 思维链基础
- [[ReAct]] — 推理与行动结合
- [[Prompt_Engineering]] — 提示工程总览
