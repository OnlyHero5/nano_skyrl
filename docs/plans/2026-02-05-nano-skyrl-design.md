# Nano‑SkyRL 设计文档（Ray + vLLM，PPO/GRPO→SAPO）

> 目标：用“系统优先”的最小架构还原 Agentic RL 关键机制，支持文本/数学任务、工具调用与简化 WebShop/AlfWorld，并可扩展到 SAPO/GSPO/DAPO。

## 1. 目标与边界
**目标**
- 构建一个“教学版”SkyRL：结构对齐大厂（agent/gym/train + Ray + vLLM），机制清晰。
- 算法路线：PPO/GRPO 基线 → SAPO；预留 GSPO/DAPO 插件口。
- 支持纯文本/数学任务 + 多工具调用环境 + 简化 WebShop/AlfWorld。
- 评测：混合 1 个 Agent 基准 + 1 个 Coding 基准。
- 后训练 Qwen Base（优先 LoRA/QLoRA 以控成本）。

**非目标**
- 不追求完整复现论文指标；先追求“闭环可跑 + 机制可解释”。
- 不引入复杂 GUI 代理或高成本多模态流程。

**约束**
- 单机 8 卡；推理由 vLLM 提供；训练可选全参或 LoRA。

## 2. 系统架构（系统优先）
**模块划分**
- `skyrl_gym/`：环境层（text_math、tool_use、webshop_lite、alfworld_lite）。
- `skyrl_agent/`：策略层（prompt 构建、工具 schema、action 解析、记忆）。
- `skyrl_train/`：训练层（rollout、trajectory、advantage、optimizer）。
- `skyrl_eval/`：评测层（agent + coding 轻量脚本）。
- `skyrl_algos/`：算法插件（PPO、GRPO、SAPO、GSPO、DAPO）。

**并行骨架（Ray + vLLM）**
- `EnvWorker`：并行环境实例；产生 obs/reward/done。
- `RolloutWorker`：调用 vLLM，生成动作并记录轨迹。
- `Learner`：消费轨迹，执行 PPO/GRPO/SAPO 更新。
- `vLLM Server`：单独进程，提供 OpenAI‑compatible API。

## 3. 数据流（核心闭环）
1) 环境 reset 生成任务 → obs
2) Agent 构建 prompt（包含工具 schema）
3) vLLM 生成 action → 解析 → env.step()
4) 记录轨迹：obs, action, reward, done, tool_calls, meta
5) 轨迹整理为 batch → 计算 advantage/return → 更新策略
6) 训练间隔触发评测与日志

**关键：** 所有“多轮工具调用”必须形成可追踪的 trajectory，才能做 RL。

## 4. 算法设计（PPO/GRPO → SAPO）
**基线 PPO/GRPO**
- 标准 token‑level 价值估计 + GAE
- GRPO 引入 group reward 归一化（多 rollout 比较）
- 统一接口：`Algorithm.update(batch, old_logprob, value, reward)`

**SAPO 插件点**
- 用连续门控替代硬裁剪：`gate = sigmoid((r - 1)/tau)`
- 目标：保持稳定更新、减少裁剪引入的偏差

**GSPO/DAPO 扩展**
- GSPO：序列级 ratio/clipping（按 sequence 汇总）
- DAPO：解耦 clip + 动态采样策略，作为采样器/优化器的组合插件

## 5. 环境设计（简化 WebShop/AlfWorld）
**text_math**
- 任务模板：算术、多步推理、逻辑题
- 奖励：答案完全匹配 + 格式正确性

**tool_use**
- 工具注册：calculator/search/wiki/python_exec
- action 格式：`<tool_call>{"tool":..., "args":...}</tool_call>`
- 奖励：工具调用是否有效 + 最终答案匹配

**webshop_lite / alfworld_lite**
- 简化为“文本交互 + 受限动作空间”
- 目标是学习“多步交互 + 工具 + 状态记忆”

## 6. 评测计划（对齐主流厂商口径）
**原则**
- 主线默认组合：HLE + SWE-bench Verified
- 主线仍保持 “1 Agent + 1 Coding”，但候选集与主流厂商报告对齐
- 能力扩展采用“可选补充”而非硬要求，避免评测负担过重

**Agent 基准（主线默认 + 备选）**
- HLE（默认）
- BrowseComp
- DeepSearchQA

**Coding 基准（主线默认 + 备选）**
- SWE-bench Verified（默认）
- SWE-bench Multilingual

**通用能力（可选补充）**
- MMLU / MMLU-Pro
- GPQA
- MATH / GSM8K
- LiveCodeBench

**落地评测脚本（最小闭环）**
- 统一 `skyrl_eval` 接口：`eval_agent.py` / `eval_code.py`
- 只跑小规模子集（<200 例）做机制验证，再扩展到完整集
- 记录指标：成功率/正确率、平均步骤、工具调用次数、token 成本

## 7. 里程碑建议
1. 搭好 Ray + vLLM + EnvWorker/ RolloutWorker/ Learner 骨架
2. 跑通 PPO/GRPO 基线在 text_math 环境
3. 加入 tool_use 环境并跑通
4. 引入 SAPO 更新策略并对比增益
5. 接入简化 WebShop/AlfWorld
6. 跑 1 agent + 1 coding 的轻量评测

## 8. 风险与验证
- **风险：** vLLM 端点与训练兼容问题 → 用 OpenAI‑compatible API 固化接口
- **风险：** 评测基准太重 → 先做 lite 子集
- **验证：** 每阶段必须能产出可视化日志（reward 曲线、success rate）

---
**说明：** 本设计是“教学版 SkyRL”。后续可按需求扩展更复杂的多模态、真实 Web/OS 环境。


