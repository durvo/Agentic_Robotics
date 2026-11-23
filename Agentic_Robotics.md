# Agentic Robotics 与 Harness VLA 资料索引

面向刚接触具身智能的本科生，聚焦桌面机械臂如何利用执行反馈发现异常，并调整动作完成任务。条目附论文、项目页或代码入口，以及与课程设计相关的阅读提示。

## 目录

- [入门路线](#入门路线)
- [研究论文与项目](#研究论文与项目)
- [仿真环境与模型](#仿真环境与模型)
- [本科课程设计方向](#本科课程设计方向)

## 入门路线

1. **先看 Inner Monologue 的演示。** 观察一次失败后，机器人收到什么反馈，又如何改变下一步动作。
2. **读 Harness VLA 的框架图。** 标出哪些动作由 VLA 生成，哪些由外层规划器调用已有技能完成，以及重试前如何调整姿态。
3. **对照 DoReMi 和 REFLECT。** 前者看异常如何被及时发现，后者看如何根据执行记录解释失败并制定恢复计划。
4. **动手跑 LeRobot 的 LIBERO 评估。** 先复现一个正常抓取任务，再在同一任务上加入可重复的物体位移。

**概念速查。** VLA（Vision–Language–Action）根据视觉和语言指令生成机器人动作。Agentic Robotics 是更广的研究方向，这份清单侧重根据目标和执行反馈调用技能、检查结果、必要时重新规划的系统。Harness VLA 是其中一篇具体论文，其外层规划器把冻结的 VLA 当作可调用和重试的操作技能。

## 研究论文与项目

| 主题 | 论文或项目 | 中文阅读说明 | 资源 |
| --- | --- | --- | --- |
| 规划与执行反馈 | **Inner Monologue**（2022） | 将任务成功判断、物体检测等结果反馈给语言模型，使规划能随执行情况调整。先看失败后重规划的演示。 | [项目与视频](https://inner-monologue.github.io/) · [论文](https://research.google/pubs/innermonologue-embodied-reasoning-through-planning-with-language-models/) |
| 异常检测 | **DoReMi**（2023 年预印本，IROS 2024） | 语言模型为计划生成约束，视觉语言模型在执行中检查约束是否被破坏。可借鉴其思路，将“是否抓稳”写成可检查的条件。 | [论文](https://arxiv.org/abs/2307.00329) · [演示](https://sites.google.com/view/doremi-paper) |
| 失败解释与修正 | **REFLECT**（CoRL 2023） | 将多传感器观察整理成执行摘要，通过语言模型解释失败并指导恢复规划。课设可以借鉴其日志设计，保存动作、观测和失败原因。 | [论文](https://proceedings.mlr.press/v229/liu23g.html) · [项目与数据集](https://robot-reflect.github.io/) · [代码](https://github.com/real-stanford/reflect) |
| 冻结 VLA 与外层规划 | **Harness VLA**（2026 年预印本，arXiv:2607.08448） | 用同一个冻结的 VLA 处理局部接触操作。外层规划器结合已有动作技能、执行反馈和记忆，负责定位、调整起始姿态、检查结果与重试。 | [论文](https://arxiv.org/abs/2607.08448) · [项目与演示](https://harnessvla.github.io/) · [RPent 文档](https://rpent.readthedocs.io/en/latest/rst_source/resources/harnessvla.html) · [代码](https://github.com/RLinf/RPent) |
| 历史与任务状态 | **SERA**（2026 年预印本） | 固定底层 VLA，由外层智能体记录任务进度和执行结果。适合延伸阅读，尤其是当前画面无法告诉机器人哪些子任务已做过的场景。 | [论文](https://arxiv.org/abs/2610.29560) |

### Harness VLA 精读提示

- **动作分工。** 定位、移到合适位置、运输和释放等环节可由已有控制技能负责。需要与物体精细接触时，由规划器调用 VLA。
- **失败恢复。** 追踪一次失败后的观测、姿态调整和再次调用，留意第二次执行的起点怎样改变。
- **记忆。** 任务记忆保存经验证的操作流程及其可调整参数，全局记忆保存可跨任务复用的成功条件、常见失败和恢复经验。可从 [RPent 的 Harness VLA 说明](https://rpent.readthedocs.io/en/latest/rst_source/resources/harnessvla.html) 进入相关文档。

## 仿真环境与模型

| 资源 | 用途 | 入口 |
| --- | --- | --- |
| **LIBERO** | 桌面操作仿真与评估。做课设可先选一个抓取放置任务。 | [官方代码](https://github.com/Lifelong-Robot-Learning/LIBERO) · [LeRobot 评估文档](https://huggingface.co/docs/lerobot/main/libero) |
| **SmolVLA** | 小型 VLA。LeRobot 提供 LIBERO 对应的模型权重，可先用现有权重完成基线评估。 | [论文](https://arxiv.org/abs/2506.01844) · [LIBERO 权重](https://huggingface.co/lerobot/smolvla_libero) · [LeRobot 代码](https://github.com/huggingface/lerobot) |
| **LIBERO-PRO** | 在 LIBERO 基础上扩展扰动评估，可参考它如何改变指令、目标关联与场景设置。 | [代码与评估说明](https://github.com/RLinf/LIBERO-PRO) |
| **VLA 论文总表** | 需要补充阅读不同类型的 VLA 时，可查阅这份更广的资料目录。 | [RobotManipulation 的 VLA 索引](https://github.com/hnuzhy/RobotManipulation/blob/main/Vision_Language_Action.md) |

## 本科课程设计方向

**题目建议** · 基于执行反馈的桌面机械臂抓取失败检测与恢复

**研究问题。** 当桌面物体在夹爪闭合前发生轻微位移，机械臂夹空后先重新对准再重试，是否比直接重试更容易完成抓取放置？

**范围。** 单人、纯仿真，选一个任务和一种扰动。使用同一份冻结的 VLA 权重。完成一段正常抓取后，再接入运行时监测和恢复动作。

1. 让 VLA 执行任务，在夹爪接近物体时施加可复现的位移。
2. 夹爪闭合并抬起后，根据物体是否随夹爪持续上升判断是否抓到。课程版可读取仿真器位姿，实验报告应说明这一观测条件。
3. 检测到夹空后，张开夹爪，退回到合适的位置，根据物体当前位置重新对准，清除尚未执行的旧动作，再让 VLA 根据最新观测继续。
4. 在相同初始场景、扰动和总步数预算下，比较原始 VLA、失败后直接重试、失败后重定位再试。两种重试方案使用相同的重试次数上限。

**验收。** 提供仿真演示、实验日志和对照结果。按正常与受扰场景报告任务成功次数与总次数，并记录首次失败后的恢复成功率及成功试次的完成步数。使用相同的场景种子，方便回看每组执行差异。

**可选扩展。** 基础版本完成后，可加入视觉语言模型，让它从预设的恢复动作中选择并与规则策略对照。若课程更强调 Harness VLA 的记忆设计，也可记录同一试次内已失败的接近方向，避免再次使用该方向。
