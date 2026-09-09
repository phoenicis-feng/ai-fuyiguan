---
title: "Aider、Claude Code 与 OpenClaw 跑同一模型，Token 消耗相差 70 倍：Agent Harness 才是成本关键"
date: 2026-09-09
author: "The New Stack / 机器之心译"
description: "同一模型、不同 Agent Harness，Token 消耗可达 70 倍差异；这意味着选择模型不如选择如何控制它——上下文压缩、重试策略、工具调用频率才是成本控制的核心。"
tags: ["Agent Harness", "Token Cost", "Aider", "Claude Code", "OpenClaw", "成本控制", "AI工程"]
categories: ["AI Agent", "AI编程"]
draft: false
---

Aider、Claude Code 与 OpenClaw 跑同一模型，Token 消耗相差 70 倍：Agent Harness 才是成本关键

> 原文：Aider, Claude Code, and OpenClaw ran an identical model. Token use varied 70-fold. — The New Stack, 2026。本文为专业中文翻译并附解读，结构重组，非逐段直译；图片为原文配图全部下载。

![Agent Harness 对比与成本差异](/img/news/agent-harness-token-costs/img01.jpg)

## 一、先看结论：不是模型贵，而是“怎么用它”贵

行业一直在比“哪个模型更强”“哪个模型更便宜”，但这篇文章的核心发现更直接：**当三个不同的 Agent Harness（Aider、Claude Code、OpenClaw）运行完全相同的底层模型时，Token 消耗差异高达 70 倍**。

这意味着，**选择模型不如选择如何控制它**。如果你把模型当作“黑箱 API”，爽用到爽；但如果你要把它嵌入真实工程工作流，Harness 的设计——上下文压缩策略、重试循环、工具调用频率、历史回溯深度——才是决定成本的真正变量。

## 二、三个 Harness 的本质差异

- **Aider**：面向开发者的 AI 结对编程工具，强调轻量级、快速迭代，通常控制上下文窗口，避免过度保留历史。
- **Claude Code**：更重工程上下文，支持多轮工具调用和长时任务，倾向保留完整会话历史以维持一致性，但代价是 Token 消耗随任务深度指数增长。
- **OpenClaw**：偏向规则驱动和结构化推理，可能在每一步都进行更严格的验证和回溯，导致单位任务的 Token 预算被放大。

同一模型、同样任务，输出质量可能接近，但**消耗路径完全不同**：Aider 可能只用 1/70 的 Token 完成同一目标，因为它主动压缩历史、减少冗余调用；而 OpenClaw 可能在每一步都保留完整推理链，确保“不会漏步骤”，但成本随之飙升。

## 三、为什么差距能到 70 倍？三个机制

**上下文压缩策略不同**：Aider 可能在达到某一阈值后主动丢弃旧上下文，用摘要替代；Claude Code 可能保留完整历史以支持跨文件引用；OpenClaw 可能要求每次推理都从完整规则集出发。这不是“好坏”之分，而是**权衡维度不同**。

**重试与验证循环**：OpenClaw 这类规则驱动 Harness 往往在每一步都做验证，失败则重试；每一次重试都消耗新 Token。Aider 更倾向“先做再修”，把修复集中在后期，减少中间重试。

**工具调用频率**：模型调用工具（搜索、执行代码、读取文件）的次数直接影响 Token。 Harness 设计决定了“何时调用工具”——有的在每轮都检索，有的只在不确定时检索，这种差异放大后就是数量级差异。

## 四、对工程团队的实际意义

这不是“选哪个 Harness 好看”的问题，而是**成本模型设计**：

- **短任务、快速原型**：Aider 式的轻量 Harness 更合适，控制成本的核心是“不要让历史无限膨胀”。
- **长周期工程项目**：Claude Code 式的深上下文更合适，但需要主动设置压缩阈值，避免“任务越长、成本越指数增长”。

关键建议：**不应把 Token 消耗当作“模型价格”问题，而应当作“Harness 配置”问题**。同一模型，在不同配置下的成本差异比换一个更便宜的模型大得多。

## 五、解读：这揭示了 AI 工程的真实成本结构

过去行业讨论“OpenAI 还是 DeepSeek 更便宜”，本质是**单次调用价格比较**；但这篇文章揭示的是**系统级成本结构**：

- 模型价格是线性的（输入/输出每百万 Token 多少）；
- Harness 效率是指数级的（上下文管理 × 工具调用 × 重试次数）。

因此，未来的成本优化不再是“选更便宜的 API”，而是**“设计更高效的 Agent Loop”**。这解释了为什么 OpenAI、Google 等都在投入 Agentic Loop 技术——不是为了“更聪明”，而是为了**让同样的钱买到更多有效推理**。

![作者与成本分析背景](/img/news/agent-harness-token-costs/img02.jpg)

## 六、结论：从“选模型”到“设计循环”

如果你正在构建 AI 工程系统，这篇文章的核心指引很清楚：

1. **先测 Harness，后选模型**：在同一模型下测试 2-3 种 Harness 配置，记录单位任务 Token 消耗。
2. **设定上下文压缩规则**：不要等“满了再压”，而是主动设定历史保留深度、摘要触发条件。

3. **控制工具调用频率**：让模型在“确定”时不搜索，在“不确定”时精准检索，而不是每轮都全面扫描。

4. **把成本当作设计指标**：和延迟、准确率一起纳入评估体系，而不是事后才算账。

一句话：**70 倍的差异不是异常，是系统设计的必然。控制它的，不是模型，而是你怎么让模型工作。**

---

**参考资料**

- 原文：Aider, Claude Code, and OpenClaw ran an identical model. Token use varied 70-fold. — The New Stack
- 作者：Pablo Merchan Montes（照片标注）
- 相关：Agent Harness 设计、Token 成本控制、AI 工程成本模型

<a href="https://thenewstack.io/agent-harness-token-costs/" target="_blank">点击查看原文</a>
