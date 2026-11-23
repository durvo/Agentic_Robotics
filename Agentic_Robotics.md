# Agentic Robotics 与 Harness VLA 资料索引




## 研究论文与项目

| 主题 | 论文或项目 | 中文阅读说明 | 资源 |
| --- | --- | --- | --- |
| 规划与执行反馈 | **Inner Monologue**（2022） | 将任务成功判断、物体检测等结果反馈给语言模型，使规划能随执行情况调整。先看失败后重规划的演示。 | [项目与视频](https://inner-monologue.github.io/) · [论文](https://research.google/pubs/innermonologue-embodied-reasoning-through-planning-with-language-models/) |
| 异常检测 | **DoReMi**（2023 年预印本，IROS 2024） | 语言模型为计划生成约束，视觉语言模型在执行中检查约束是否被破坏。可借鉴其思路，将“是否抓稳”写成可检查的条件。 | [论文](https://arxiv.org/abs/2307.00329) · [演示](https://sites.google.com/view/doremi-paper) |
| 失败解释与修正 | **REFLECT**（CoRL 2023） | 将多传感器观察整理成执行摘要，通过语言模型解释失败并指导恢复规划。课设可以借鉴其日志设计，保存动作、观测和失败原因。 | [论文](https://proceedings.mlr.press/v229/liu23g.html) · [项目与数据集](https://robot-reflect.github.io/) · [代码](https://github.com/real-stanford/reflect) |
| 冻结 VLA 与外层规划 | **Harness VLA**（2026 年预印本，arXiv:2607.08448） | 用同一个冻结的 VLA 处理局部接触操作。外层规划器结合已有动作技能、执行反馈和记忆，负责定位、调整起始姿态、检查结果与重试。 | [论文](https://arxiv.org/abs/2607.08448) · [项目与演示](https://harnessvla.github.io/) · [RPent 文档](https://rpent.readthedocs.io/en/latest/rst_source/resources/harnessvla.html) · [代码](https://github.com/RLinf/RPent) |
| 历史与任务状态 | **SERA**（2026 年预印本） | 固定底层 VLA，由外层智能体记录任务进度和执行结果。适合延伸阅读，尤其是当前画面无法告诉机器人哪些子任务已做过的场景。 | [论文](https://arxiv.org/abs/2610.29560) |
