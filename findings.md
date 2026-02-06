# Findings & Decisions
<!-- 
  WHAT: Your knowledge base for the task. Stores everything you discover and decide.
  WHY: Context windows are limited. This file is your "external memory" - persistent and unlimited.
  WHEN: Update after ANY discovery, especially after 2 view/browser/search operations (2-Action Rule).
-->

## Requirements
<!-- 
  WHAT: What the user asked for, broken down into specific requirements.
  WHY: Keeps requirements visible so you don't forget what you're building.
  WHEN: Fill this in during Phase 1 (Requirements & Discovery).
-->
<!-- Captured from user request -->
- 系统优先：Ray + vLLM + 类 verl 的训练骨架
- 算法路线：先 PPO/GRPO 基线，再加 SAPO，保留 GSPO/DAPO 接口
- 环境：纯文本/数学任务 + 多工具调用 + 简化 WebShop/AlfWorld
- 评测：混合 1 个 Agent 基准 + 1 个 Coding 基准
- 主线默认组合：HLE + SWE-bench Verified
- 产出：中文设计文档，写在 nano_SkyRL 目录中

## Research Findings
<!-- 
  WHAT: Key discoveries from web searches, documentation reading, or exploration.
  WHY: Multimodal content (images, browser results) doesn't persist. Write it down immediately.
  WHEN: After EVERY 2 view/browser/search operations, update this section (2-Action Rule).
-->
<!-- Key discoveries during exploration -->
- 主线默认评测组合已固定为 HLE + SWE-bench Verified
- 评测对齐主流厂商口径：Agent 基准优先 HLE / BrowseComp / DeepSearchQA（择一）；Coding 基准优先 SWE-bench Verified / SWE-bench Multilingual（择一）。\n- 通用能力可选补充：MMLU、MMLU-Pro、GPQA、MATH、GSM8K、LiveCodeBench（视资源与时间）

## Technical Decisions
<!-- 
  WHAT: Architecture and implementation choices you've made, with reasoning.
  WHY: You'll forget why you chose a technology or approach. This table preserves that knowledge.
  WHEN: Update whenever you make a significant technical choice.
-->
<!-- Decisions made with rationale -->
| Decision | Rationale |
|----------|-----------|
| 系统优先骨架（Ray + vLLM） | 对齐大厂工程结构，利于理解并行训练 |
| 算法顺序：PPO/GRPO→SAPO | 先稳定基线，再验证增益 |
| 评测对齐主流厂商基准 | 方便对照行业公开榜单与模型报告 |
| 主线默认评测组合：HLE + SWE-bench Verified | 固定主线，便于复现实验 |

## Issues Encountered
<!-- 
  WHAT: Problems you ran into and how you solved them.
  WHY: Similar to errors in task_plan.md, but focused on broader issues (not just code errors).
  WHEN: Document when you encounter blockers or unexpected challenges.
-->
<!-- Errors and how they were resolved -->
| Issue | Resolution |
|-------|------------|
| 无法提交设计文档 | nano_SkyRL 目录无 .git，未执行提交 |
| GitHub CLI 未安装（无法自动创建远端仓库） | 计划改用 GitHub Web 或 API + PAT |

## Resources
<!-- 
  WHAT: URLs, file paths, API references, documentation links you've found useful.
  WHY: Easy reference for later. Don't lose important links in context.
  WHEN: Add as you discover useful resources.
-->
<!-- URLs, file paths, API references -->
- Project root: `H:\src\nano_SkyRL`
- Local git initialized: `H:\src\nano_SkyRL\.git`
- Status doc: `H:\src\nano_SkyRL\docs\STATUS.md`
- Benchmarks (对齐主流口径): HLE, BrowseComp, DeepSearchQA, SWE-bench Verified, SWE-bench Multilingual

## Visual/Browser Findings
<!-- 
  WHAT: Information you learned from viewing images, PDFs, or browser results.
  WHY: CRITICAL - Visual/multimodal content doesn't persist in context. Must be captured as text.
  WHEN: IMMEDIATELY after viewing images or browser results. Don't wait!
-->
<!-- CRITICAL: Update after every 2 view/browser operations -->
<!-- Multimodal content must be captured as text immediately -->
-

---
<!-- 
  REMINDER: The 2-Action Rule
  After every 2 view/browser/search operations, you MUST update this file.
  This prevents visual information from being lost when context resets.
-->
*Update this file after every 2 view/browser/search operations*
*This prevents visual information from being lost*





