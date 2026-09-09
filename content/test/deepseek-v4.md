---
title: "花了4千万token实测 DeepSeek V4：长上下文、Agent 编码与国产化"
date: 2026-04-24
author: "苍何"
description: "前后砸了约4000万token，实测 DeepSeek V4-Pro / Flash：1M上下文标配、Agent 编码有进步但仍有坑、推理知识逼近顶级闭源、底层全面拥抱国产化。"
tags: ["DeepSeek V4", "长上下文", "Agent Coding", "国产化", "AI实测", "开源模型"]
categories: ["深度评测"]
draft: false
---

花了4千万token实测 DeepSeek V4：长上下文、Agent 编码与国产化

> 测试背景：作者前后投入约4000万token，覆盖长文本重构、前端工程（Wesight）、Agent Loop、代码构建、推理与写作等场景；对比内容包括 GLM 5.1、Kimi K2.6、Sonnet 4.5 / Opus 4.6 及 Code Arena 公开评测。

## 一、版本定位：V4-Pro 与 V4-Flash

DeepSeek V4 分成两个版本，定位很清晰：

- **V4-Pro**：1.6T 总参数，49B 激活，1M 上下文。旗舰对标顶级闭源模型。
- **V4-Flash**：284B 总参数，13B 激活，同样 1M 上下文。主打便宜与快。

技术报告显示：V4 从底层围绕长上下文设计，1M 不再是“标配噱头”，而是可真实用满的架构默认。

## 二、架构升级：混合注意力 + Muon + 国产化内核

核心改动有三点，直接影响长文本连贯性：

1. **Hybrid Attention**：交错使用 CSA（Compressed Sparse Attention）与 HCA（Heavily Compressed Attention），一套管长距离依赖，一套管超长压缩——相当于给模型装了“近清远不模糊”的两套眼睛。
2. **优化器换 Muon**：收敛更快更稳；配合流形约束残差连接，参数调度更灵活。
3. **国产化底层**：引入 MXFP4（适配华为昇腾、寒武纪、壁仞）、TileLang（DSL → 多硬件编译，降低 CUDA 依赖）、MegaMoE（减少专家并行通信等待，已在昇腾跑通）。

实测验证：喂入接近90万token的代码库要求全局重构，V3.2 干到一半就开始“忘事”，变量名对不上；V4-Pro 几乎全程在线，跨越数十万token仍能记住之前定的命名规范。

![架构与注意力示意](/img/test/deepseek-v4/03-arch-attention.png)

## 三、编码能力：有进步，工程上下文仍差一截

### 3.1 前端与 Demo：审美有明显增强

一句提示词生成个人博客、Wesight 登录页优化，科技感拉满；但 Demo 与工程代码是两码事。Wesight 涉及 Electron 构建、多引擎调度、Node 原生模块编译，模块耦合度高，对模型工程理解力要求完全不同量级。

### 3.2 工程构建：同样错误会反复犯

在 Electron 构建场景，GLM 5.1 基本一轮定位根因、改完不复发；V4 是“改了犯、犯了改”，同一配置项反复横跳。这说明差距不在语法，在工程上下文追踪深度上。

![Wesight 构建问题示例](/img/test/deepseek-v4/04-coding-wesight.png)

### 3.3 Agent 循环：简单任务流畅，复杂任务易中断

用 V4-Pro + Claude Code：简单到中等难度任务很流畅，代码风格偏实战、不过度设计，这在 Agentic Coding 场景反而是优点。但存在三个硬伤：

- **本地 skill 调用不敏感**：GLM 5.1 / Kimi K2.6 能立刻识别并调用 tool，V4 常需要明确提示才动，决策“不够果断”。
- **复杂约束下易跳过**：例如“提交前必须过 commit 自检”，V4 直接跳过约束推代码；若不盯着完全不知道漏了哪一步，这在团队协作里是一票否决级风险。GLM 5.1 按 checklist 一步不落走完，稳得多。
- **长时任务自己停**：不是报错也不是超时，就是单纯中断不继续，无法挂后台，只能旁边盯着催。实际工程中很难受，最后还是切回 GLM 5.1 一次解决。

![Agent 技能与构建对比](/img/test/deepseek-v4/06-agent-skill.png)

## 四、Agent 能力：进步显著，但还不是顶级

官方内部评测说 V4-Pro “体验优于 Sonnet 4.5，交付质量接近 Opus 4.6 非思考模式”。实测接近：简单/中等任务流畅，风格实战；但在复杂工程上下文、约束遵守、长任务稳定性上，仍与 GLM 5.1 有明确差距。

Code Arena 测试显示：V4-Pro 相较 V3.2 进步很大，但仍次于 GLM 5.1 和 Kimi K2.6；与作者实测结果一致。

![Code Arena 对比与构建报错](/img/test/deepseek-v4/05-electron-error.png)

## 五、推理与知识：数学/STEM 霸榜，冷门知识准确率上一个档

- **推理**：在数学、STEM、竞赛型代码上，超越所有已公开评测的开源模型，跟世界顶级闭源打平。
- **世界知识**：大幅领先其他开源模型，仅略逊 Gemini 3.1 Pro；冷门编程语言特性、特定年份学术论文细节，准确率比 V3.2 高一个档次。经典“洗车问题”回答准确。
- **多模态**：目前**纯文本**，官方明确暂不放出，可能等 V4.5 / V5.0。

![推理与知识演示](/img/test/deepseek-v4/09-reasoning-demo.png)

## 六、写作能力：风格化不错，适合长文本仿写

配合 Obsidian 知识库，基于 V4 强大上下文写作，风格学习很到位，接近 Claude Opus 4.6，好过 4.7；素材越丰富，仿写味道越对味。对长文本分析与风格化写作，V4 绝对值得一试。

![写作风格与知识库演示](/img/test/deepseek-v4/10-writing-style.png)

## 七、价格与国产化：涨了但依然良心，且有下行空间

| 模型 | 输入 | 输出 |
|---|---|---|
| V4-Pro | 12 元/百万 token | 24 元/百万 token |
| V4-Flash | 1 元/百万 token | 2 元/百万 token |

相比 V3 确实涨了，但参数量 1.6T 是 V3.2 近 2.5 倍；横向对比海外（Claude Opus 4.7 25/25，GPT-5.5 约 30/30），国产整体仍便宜约 60%。更关键的是：下半年华为昇腾 950 超节点批量上市后，V4-Pro 定价预计大幅下调——现在更像“产能不够，用价格控流量”。

![价格与对比概览](/img/test/deepseek-v4/08-price-table.png)

## 八、总结：V4 是宣言书，不是碾压式领先

- **不是**让你“卧槽牛逼”的模型：没有碾压式领先，没有革命性新功能。
- **是**让你“方向对了”的模型：1M 上下文标配、Agent 能力大幅提升、推理知识逼近顶级闭源、底层架构全面拥抱国产化，每一点都在为未来铺路。

一句话评价：**“V3 是 DeepSeek 的成人礼，V4 是 DeepSeek 的宣言书。”**

如果主攻复杂工程开发、重度依赖 Agent Coding，现阶段用国产模型 GLM 5.1 仍更稳；但做长文本分析、知识问答、风格化写作，V4 绝对值得一试。

![实测总结与选择建议](/img/test/deepseek-v4/02-versions.png)

---

**参考资料**

1. [DeepSeek-V4 官方发文](https://mp.weixin.qq.com/s/8bxXqS2R8Fx5-1TLDBiEDg?scene=1&click_id=9)
2. [DeepSeek-V4 技术报告 PDF](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro/blob/main/DeepSeek_V4.pdf)
3. [赛博禅心公众号](https://mp.weixin.qq.com/s/mjaBklBlAhUF4AXvVgMo1A)
4. [卡兹克公众号](https://mp.weixin.qq.com/s/HBh2sRbJwDPB1L0lZ6nzHg)
5. [DeepSeek API 文档](https://api-docs.deepseek.com/zh-cn/guides/coding_agents)

<a href="https://www.aixq.cc/24138.html" target="_blank">点击查看原文</a>
