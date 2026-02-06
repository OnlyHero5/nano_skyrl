# Nano-SkyRL 项目状态（会话快速恢复）

更新时间：2026-02-05

## 项目目标（极简）
构建“教学版”Agentic RL 全栈框架（Ray + vLLM），先跑通 PPO/GRPO 基线，再加入 SAPO（预留 GSPO/DAPO），支持文本/数学 + 工具调用 + 简化 WebShop/AlfWorld，并在主流厂商常用基准上做对齐评测。

## 当前进度
- 设计文档已完成并更新评测对齐方案
- 当前阶段：Phase 5（交付完成，等待实施阶段）

## 评测对齐（主线口径）
**主线默认**：HLE + SWE-bench Verified
**Agent（择一）**：HLE / BrowseComp / DeepSearchQA
**Coding（择一）**：SWE-bench Verified / SWE-bench Multilingual
**可选补充**：MMLU、MMLU-Pro、GPQA、MATH、GSM8K、LiveCodeBench

## 新会话快速入口（建议阅读顺序）
1. docs/STATUS.md（本文件）
2. docs/plans/2026-02-05-nano-skyrl-design.md（整体设计与架构）
3. task_plan.md（任务路线与阶段）
4. findings.md（关键发现与决策）
5. progress.md（执行日志与变更）

## 已确认关键决策
- 系统优先骨架：Ray + vLLM
- 算法路线：PPO/GRPO → SAPO，预留 GSPO/DAPO
- 评测口径：与主流厂商公开基准对齐

## 下一步（进入实现时）
1. 建立 Ray + vLLM 最小训练骨架（EnvWorker/RolloutWorker/Learner）
2. 跑通 text_math 的 PPO/GRPO 基线闭环
3. 接入 tool_use 环境并完成最小评测脚本

## 风险提示
- 评测基准可能存在数据获取与运行成本问题，需要按“子集 → 全量”推进
- vLLM 推理端点与训练框架耦合度需提前验证



