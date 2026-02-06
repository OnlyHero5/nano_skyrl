# Task Plan: Nano-SkyRL 设计文档（Ray + vLLM + PPO/GRPO→SAPO）
<!-- 
  WHAT: This is your roadmap for the entire task. Think of it as your "working memory on disk."
  WHY: After 50+ tool calls, your original goals can get forgotten. This file keeps them fresh.
  WHEN: Create this FIRST, before starting any work. Update after each phase completes.
-->

## Goal
<!-- 
  WHAT: One clear sentence describing what you're trying to achieve.
  WHY: This is your north star. Re-reading this keeps you focused on the end state.
  EXAMPLE: "Create a Python CLI todo app with add, list, and delete functionality."
-->
在 `nano_SkyRL` 目录内输出一份中文设计文档，描述系统架构、数据流、算法扩展（PPO/GRPO→SAPO/GSPO/DAPO）、环境与评测方案（1 Agent + 1 Coding 基准）。

## Current Phase
<!-- 
  WHAT: Which phase you're currently working on (e.g., "Phase 1", "Phase 3").
  WHY: Quick reference for where you are in the task. Update this as you progress.
-->
Phase 5

## Phases
<!-- 
  WHAT: Break your task into 3-7 logical phases. Each phase should be completable.
  WHY: Breaking work into phases prevents overwhelm and makes progress visible.
  WHEN: Update status after completing each phase: pending → in_progress → complete
-->

### Phase 1: Requirements & Discovery
<!-- 
  WHAT: Understand what needs to be done and gather initial information.
  WHY: Starting without understanding leads to wasted effort. This phase prevents that.
-->
- [x] 确认学习目标与约束（系统优先、并行、可扩展算法）
- [x] 明确环境类型（文本/数学 + 工具调用 + WebShop/AlfWorld 简化）
- [x] 明确评测范围（1 Agent + 1 Coding）
- **Status:** complete
<!-- 
  STATUS VALUES:
  - pending: Not started yet
  - in_progress: Currently working on this
  - complete: Finished this phase
-->

### Phase 2: Architecture Draft
<!-- 
  WHAT: Decide how you'll approach the problem and what structure you'll use.
  WHY: Good planning prevents rework. Document decisions so you remember why you chose them.
-->
- [x] 定义模块边界（agent/gym/train）
- [x] 设计 Ray Actor 拓扑与 vLLM 服务流
- [x] 记录关键设计权衡
- **Status:** complete

### Phase 3: Algorithm & Env Spec
<!-- 
  WHAT: Actually build/create/write the solution.
  WHY: This is where the work happens. Break into smaller sub-tasks if needed.
-->
- [x] 定义 PPO/GRPO 基线与 SAPO 扩展接口
- [x] 定义 tool-calling 与简化 WebShop/AlfWorld 环境接口
- [x] 定义奖励与日志规范
- **Status:** complete

### Phase 4: Evaluation Plan
<!-- 
  WHAT: Verify everything works and meets requirements.
  WHY: Catching issues early saves time. Document test results in progress.md.
-->
- [x] 选择 1 个 Agent 基准 + 1 个 Coding 基准
- [x] 设计最小评测脚本与指标
- [x] 明确单机 8 卡规模假设
- **Status:** complete

### Phase 5: Delivery
<!-- 
  WHAT: Final review and handoff to user.
  WHY: Ensures nothing is forgotten and deliverables are complete.
-->
- [x] 写出中文设计文档（docs/plans/）
- [x] 校对完整性与可读性
- [x] 交付给用户
- **Status:** complete

## Key Questions
<!-- 
  WHAT: Important questions you need to answer during the task.
  WHY: These guide your research and decision-making. Answer them as you go.
  EXAMPLE: 
    1. Should tasks persist between sessions? (Yes - need file storage)
    2. What format for storing tasks? (JSON file)
-->
1. 系统骨架如何最小化但仍体现大厂风格？
2. 评测与训练的最小闭环如何定义？
3. SAPO/GSPO/DAPO 的接口如何统一？

## Decisions Made
<!-- 
  WHAT: Technical and design decisions you've made, with the reasoning behind them.
  WHY: You'll forget why you made choices. This table helps you remember and justify decisions.
  WHEN: Update whenever you make a significant choice (technology, approach, structure).
  EXAMPLE:
    | Use JSON for storage | Simple, human-readable, built-in Python support |
-->
| Decision | Rationale |
|----------|-----------|
| 系统优先（Ray + vLLM） | 对齐工程结构、理解并行训练 |
| 评测对齐主流厂商口径 | 便于与公开榜单与模型报告对照 |
| 主线默认评测组合：HLE + SWE-bench Verified | 固定主线，便于复现实验 |

## Errors Encountered
<!-- 
  WHAT: Every error you encounter, what attempt number it was, and how you resolved it.
  WHY: Logging errors prevents repeating the same mistakes. This is critical for learning.
  WHEN: Add immediately when an error occurs, even if you fix it quickly.
  EXAMPLE:
    | FileNotFoundError | 1 | Check if file exists, create empty list if not |
    | JSONDecodeError | 2 | Handle empty file case explicitly |
-->
| Error | Attempt | Resolution |
|-------|---------|------------|
| session-catchup.py not found in `.claude` path | 1 | 使用 `.codex` 目录下的脚本路径执行 |
| PowerShell ParserError when inserting status doc line | 1 | 改用单引号避免反引号转义 |
| gh CLI not installed (cannot create repo via gh) | 1 | 改用 GitHub Web 或 API + PAT |
| git ls-remote failed (Host key verification failed) | 1 | 需要将 github.com 加入 known_hosts |
| ssh -T git@github.com timed out | 1 | 网络/SSH 配置待确认，可能阻塞推送 |
| ssh keyscan failed (unsupported KEX) | 1 | 改用 ssh -o KexAlgorithms 直连并 accept-new |
| git push failed (Repository not found) | 1 | 需在 GitHub 创建仓库 nano_skyrl |
| git push succeeded (origin/main) | 1 | 完成远端推送 |

## Notes
<!-- 
  REMINDERS:
  - Update phase status as you progress: pending → in_progress → complete
  - Re-read this plan before major decisions (attention manipulation)
  - Log ALL errors - they help avoid repetition
  - Never repeat a failed action - mutate your approach instead
-->
- Update phase status as you progress: pending → in_progress → complete
- Re-read this plan before major decisions (attention manipulation)
- Log ALL errors - they help avoid repetition








