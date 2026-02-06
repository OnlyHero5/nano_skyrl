# nano_skyrl

[![项目阶段](https://img.shields.io/badge/阶段-设计完成%20待实现-blue)](docs/STATUS.md)
[![语言](https://img.shields.io/badge/语言-中文-important)](#)
[![RL栈](https://img.shields.io/badge/RL-Ray%20%2B%20vLLM-success)](docs/plans/2026-02-05-nano-skyrl-design.md)
[![算法](https://img.shields.io/badge/算法-PPO%20%7C%20GRPO%20%7C%20SAPO%20%7C%20GSPO%20%7C%20DAPO-informational)](docs/plans/2026-02-05-nano-skyrl-design.md)
[![默认评测](https://img.shields.io/badge/默认评测-HLE%20%2B%20SWE--bench%20Verified-orange)](docs/STATUS.md)
[![分支](https://img.shields.io/badge/branch-main-black)](../../tree/main)

一个面向学习和复刻的 Agentic RL 教学项目：系统优先，先跑通端到端闭环，再逐步扩展算法和评测。

> [!IMPORTANT]
> 当前仓库以架构设计与实施路线为主，代码实现还未开始。主线目标是构建可运行的 `Ray + vLLM` 训练骨架，并支持 `PPO/GRPO -> SAPO`。

## 项目标签

`#AgenticRL` `#Ray` `#vLLM` `#PPO` `#GRPO` `#SAPO` `#GSPO` `#DAPO` `#ToolCalling` `#Qwen`

## 目标范围

- 系统骨架：`skyrl_agent` + `skyrl_gym` + `skyrl_train` + `skyrl_eval` + `skyrl_algos`
- 算法路线：`PPO/GRPO` 基线先落地，再接入 `SAPO`，预留 `GSPO/DAPO` 插件位
- 环境类型：纯文本/数学、多工具调用、简化 `WebShop/AlfWorld`
- 评测主线：`HLE + SWE-bench Verified`
- 资源约束：单机 `8 GPU`

## 能力矩阵

| 模块 | 目标状态 | 当前状态 | 备注 |
|---|---|---|---|
| 并行训练骨架（Ray + vLLM） | 可运行 | 设计完成 | 待实现 |
| 算法（PPO/GRPO） | 可训练 | 设计完成 | 待实现 |
| 算法（SAPO/GSPO/DAPO） | 插件化扩展 | 设计完成 | 待实现 |
| 环境（text_math/tool_use） | 可交互 | 设计完成 | 待实现 |
| 环境（webshop_lite/alfworld_lite） | 可交互 | 设计完成 | 待实现 |
| 评测（HLE + SWE-bench Verified） | 可复现实验 | 设计完成 | 待实现 |

## 推荐目录结构

```text
nano_SkyRL/
├─ docs/
│  ├─ STATUS.md
│  └─ plans/
│     └─ 2026-02-05-nano-skyrl-design.md
├─ skyrl_agent/
├─ skyrl_gym/
├─ skyrl_train/
├─ skyrl_eval/
├─ skyrl_algos/
├─ findings.md
├─ progress.md
├─ task_plan.md
└─ README.md
```

## 评测口径

> [!NOTE]
> 主线默认组合固定为 `HLE + SWE-bench Verified`，用于对齐主流模型公开报告常见展示方式。

- Agent 备选：`BrowseComp`、`DeepSearchQA`
- Coding 备选：`SWE-bench Multilingual`
- 可选补充：`MMLU`、`MMLU-Pro`、`GPQA`、`MATH`、`GSM8K`、`LiveCodeBench`

## 快速上手（当前阶段）

1. 阅读项目状态：`docs/STATUS.md`
2. 阅读设计文档：`docs/plans/2026-02-05-nano-skyrl-design.md`
3. 查看执行轨迹：`task_plan.md`、`findings.md`、`progress.md`
4. 按里程碑进入实现：先搭骨架，再接算法，再接评测

## 里程碑

1. 搭建 `EnvWorker / RolloutWorker / Learner` 最小闭环
2. 跑通 `text_math` 上的 `PPO/GRPO`
3. 接入 `tool_use` 环境与轨迹记录
4. 引入 `SAPO` 并与基线对比
5. 加入 `webshop_lite/alfworld_lite`
6. 跑通 `HLE + SWE-bench Verified` 轻量子集

## 风险与约束

> [!WARNING]
> 评测基准较重，建议先跑子集（例如 `<200` 样本）验证训练和评测闭环，再逐步扩展到完整集。

- 训练与推理接口兼容性：优先固定 OpenAI-compatible API
- 成本控制：优先 LoRA/QLoRA，避免一开始就全参训练
- 数据闭环：工具调用轨迹必须可追踪、可回放、可训练

## 文档索引

- 项目状态：`docs/STATUS.md`
- 系统设计：`docs/plans/2026-02-05-nano-skyrl-design.md`
- 任务规划：`task_plan.md`
- 关键发现：`findings.md`
- 进度记录：`progress.md`

> [!TIP]
> 新开对话时，先贴 `docs/STATUS.md` 与设计文档路径，能最快恢复上下文。
