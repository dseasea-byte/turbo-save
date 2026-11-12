# Turbo Save · 快省双全

> 省成本且提速的批量任务分发工作流。把批量任务拆成「强模型规划 + 低成本模型并行执行」。
> 作者：董海洋（DHY.sea）

[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-SKILL.md-blue)](https://agentskills.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 这是什么

大多数 Agent 在批量任务上亏钱：用最贵的模型逐条干最机械的活。

Turbo Save 把这种情况拆成两层——**强模型只做规划与验收，执行交给最便宜的可选模型，按批并行**。它不是提示词技巧，是一套带门禁的判断流程：不满足条件就不拆，因为拆错比不拆更贵。

## 装上之前，先看这张表

省钱和提速是两个独立的杠杆，依赖两种不同的能力。缺一个，就只拿一半。

| 能降档到更便宜的模型 | 能并行派发 | 你能拿到的收益 |
|---|---|---|
| 是 | 是 | 省成本 50% 至 85% **＋** 提速 4 至 6 倍（实测约 6 倍） |
| 是 | 否 | 省成本 50% 至 85%，不提速 |
| 否 | 是 | 只提速，倍率受同账号速率限制，**低于 6 倍，未实测**；另得上下文隔离收益 |
| 否 | 否 | 无收益，**不要安装** |

**省钱只取决于能不能降档，提速只取决于能不能并行。** 单模型用户唯一稳定的结构收益是「把一次巨型上下文拆成多次小上下文」，主线程只收摘要，不会被几十份产出撑爆或截断。

## 优势

1. **双收益可调。** 并行换时间，串行换成本。按任务性质取其一，不承诺两头同时拉满。
2. **三道门禁防亏。** 规模门槛、可判性、可切分性，任一不过就不拆。
3. **先写验收再降档。** 降档前必须交出机器可验证的验收标准，杜绝产物看着对、其实跑偏。
4. **定点打回，不整批重跑。** 返工只重跑坏的那几份。
5. **跨 Agent 通用。** 只依赖「切分 / 并行 / 验收」三个动作，不依赖特定工具名、模型名或计费体系。
6. **结束即报账。** 三行交代执行档位、主模型介入次数、节省来源。

## 性能

用例：10 份、每份 600 字的中文批量改写任务。

| 项目 | 数据 | 口径 |
|---|---|---|
| 串行基线 | 113.0 秒 | 全程使用当前会话模型逐份生成 |
| 并行执行 | 18.8 秒 | 10 路并发，执行层降档 |
| 提速 | 约 6.0 倍 | 实测，同机同任务 |
| 成本下降 | 80% 至 85% | 抽检验收、单批产出量大 |
| 成本下降 | 50% 至 60% | 逐条全读验收 |
| 8 路并发提速 | 约 5 倍 | 按拥塞趋势推算，非实测 |

两条硬边界：两个收益不可同时取满；所有比例均相对于当前会话所选模型，会话已在最低成本档时收益趋近于零。

## 什么时候用

**适用**：任务可切成可重复单元（批量改写、逐条检查、字段提取、多份分别生成、大批量整理），且产物对错可被机器判定。

**不适用**：单一连续产出、需要整体立意判断、或用户明确要求「用最好的，别省」。

## 安装

**SkillHub（国内加速）**

```bash
skillhub --dir ~/.workbuddy/skills install turbo-save
```

**手动安装（任意 Agent）**

把本仓库的 `SKILL.md` 放进对应 Agent 的技能目录即可：

| Agent | 目录 |
|---|---|
| WorkBuddy / CodeBuddy | `~/.workbuddy/skills/turbo-save/` |
| Claude Code | `~/.claude/skills/turbo-save/` |
| Cursor | `~/.cursor/skills/turbo-save/` |
| Codex | `~/.codex/skills/turbo-save/` |
| Gemini CLI | `~/.gemini/skills/turbo-save/` |

## English Overview

**Turbo Save** turns a batch job into two layers: the strong model plans and inspects, while the cheapest available model does the bulk work in parallel batches. On suitable workloads it cuts execution cost by 50–85% and wall-clock time by 4–6×.

**Requirements.** Cost savings need a cheaper tier to downgrade to; speed needs parallel dispatch. With a single model and no parallelism, this skill yields nothing. With a single model but parallelism, expect speed and context isolation only, below 6×.

It is a decision procedure, not a prompt trick. Three gates — size, verifiability, separability — must all pass before a task is split, because splitting wrongly costs more than not splitting at all.

**Advantages.** Two independent levers (parallelism buys speed, serial downshifting buys cost); acceptance criteria written before any downgrade; targeted rejections instead of whole-batch reruns; no dependency on a specific tool, model, or billing unit; a three-line ledger at the end of every run.

**Measured.** Ten 600-character Chinese rewriting tasks: 113.0 s serially, 18.8 s with 10-way parallel execution on a cheaper tier — about 6.0× faster.

**Caveats.** Gains are relative to the model already selected in the session. Parallelism and cost savings trade against each other. If the session already runs on the cheapest tier, expect near-zero benefit.

## 文件结构

```
.
├── SKILL.md      # 技能定义与全部判据（Agent 读取此文件）
├── README.md     # 本文件，面向人
└── LICENSE       # MIT
```

## 许可证

MIT © 董海洋（DHY.sea）
