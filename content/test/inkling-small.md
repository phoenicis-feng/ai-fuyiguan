---
title: "小号美国最强开源模型：Inkling-Small 登场，约 1/4 规模提供相近 AI 性能"
date: 2026-09-08
author: "弗一"
description: "Thinking Machines Lab（TML）第二款模型 Inkling-Small 面世，立刻引发行业讨论。"
tags: ["Inkling-Small","Thinking Machines Lab","MoE","开源模型","AI编程","技术评估"]
draft: false
---

## 一、概述

Thinking Machines Lab 昨日（7 月 30 日）发布博文，宣布推出 Inkling-Small 模型，其性能媲美 Inkling，但规模降低至约四分之一。

IT之家此前报道，Inkling 号称“美国最强开源 AI 模型”，采用混合专家（MoE）架构，总参数 975B、激活参数 41B，最长上下文 1M tokens。

而本次推出的 Inkling-Small 模型同样采用混合专家（MoE）架构，总参数为 276B，激活参数为 12B，规模约为 Inkling 的 1/4，在 NVIDIA GB300 NVL72 系统上训练。

<div markdown="1" style="display:flex; gap:0.5rem;">
<img style="max-width:280px; height:auto;" src="/img/inkling-small/1.png" alt="亮点" style="max-width:92%; height:auto; display:inline-block; vertical-align:top; margin:0.5rem 0.3rem;">
<img style="max-width:280px; height:auto;" src="/img/inkling-small/2.png" alt="亮点" style="max-width:92%; height:auto; display:inline-block; vertical-align:top; margin:0.5rem 0.3rem;">
<img style="max-width:280px; height:auto;" src="/img/inkling-small/3.png" alt="亮点" style="max-width:92%; height:auto; display:inline-block; vertical-align:top; margin:0.5rem 0.3rem;">
</div>

“小号”美国最强开源模型：Inkling-Small 登场，约 1/4 规模提供相近 AI 性能

<div markdown="1" style="display:flex; gap:0.5rem;">
<img style="max-width:280px; height:auto;" src="/img/inkling-small/1.avif" alt="对比" style="max-width:92%; height:auto; display:inline-block; vertical-align:top; margin:0.5rem 0.3rem;">
<img style="max-width:280px; height:auto;" src="/img/inkling-small/2.avif" alt="对比" style="max-width:92%; height:auto; display:inline-block; vertical-align:top; margin:0.5rem 0.3rem;">
</div>

Thinking Machines 已开放 Inkling-Small 完整权重，同时将其接入 Tinker 微调服务，并在 Tinker Playground 提供文本、图像和音频聊天体验。


Thinking Machines 称，Inkling-Small 以更低计算量取得接近 Inkling 的表现。在智能体工具使用、推理和指令遵循测试中，Inkling-Small 的效率高于 Inkling，并可与同等权重级别的其他模型竞争。


在推理和智能体任务中，Inkling-Small 匹配或超过 Inkling。Humanity’s Last Exam 测试中，Inkling-Small 得分 31.6%，高于 Inkling 的 29.7%。Thinking Machines 称，在每个思考预算下，Inkling-Small 的测试时计算曲线均高于 Inkling。

<div markdown="1" style="display:grid; grid-template-columns: 1fr 1fr; gap:1rem;">
<img style="max-width:280px; height:auto;" src="/img/inkling-small/3.avif" alt="对比" style="max-width:92%; height:auto; display:inline-block; vertical-align:top; margin:0.5rem 0.3rem;">
<img style="max-width:280px; height:auto;" src="/img/inkling-small/4.avif" alt="对比" style="max-width:92%; height:auto; display:inline-block; vertical-align:top; margin:0.5rem 0.3rem;">
</div>

## 二、性能与成本要点

Inkling Small 的智能水平处于领先行列，价格也较为合理；比较对象为其他规模相近的开放权重模型。此外，其速度明显较快，但回答有些冗长。该模型支持文本、图像和语音输入，可输出文本，上下文窗口为 1M 个 token。

Inkling Small 在 Artificial Analysis Intelligence Index 上的得分为 26，在同类模型中远高于平均水平（中位数：17）。在 Intelligence Index 评测中，它生成了 140M 个 token；与 120M 的中位数相比，其回答有些冗长。

Inkling Small 每 100 万输入 token 的价格为 $0.30（中等，中位数：$0.30），每 100 万输出 token 的价格为 $1.20（有些昂贵，中位数：$1.16）。在 Intelligence Index 上评测 Inkling Small 的总成本为 $240.85。

Inkling Small 的速度为每秒 126 个 token，明显较快（70）。

目前，Inkling-Small 已在 openrouter 中发布 free 版本。