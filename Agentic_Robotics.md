# VLA 机械臂抓取失败检测与反馈恢复论文清单

**研究问题**　桌面上的目标在夹爪接近时发生偏移。机械臂能否从试抬过程判断目标是否被抓起？如果第一次抓空，在同样只允许补抓一次的条件下，显式重新定位能比直接重试多救回多少次抓取？

共 15 篇。按实验环节编排，每篇给出英文题名、发表会议或所引版本、原文入口及中文阅读提示。带 **★** 的 5 篇适合优先精读。带 **◇** 的条目也收录在参考仓库中。[参考仓库 · Vision Language Action](https://github.com/hnuzhy/RobotManipulation/blob/main/Vision_Language_Action.md)

## 目录

- [目标偏移与抓取结果验证](#目标偏移与抓取结果验证)
- [失败后的重抓与动作修正](#失败后的重抓与动作修正)
- [VLA 执行监测与基础策略](#vla-执行监测与基础策略)
- [重试基线与恢复评测](#重试基线与恢复评测)

## 目标偏移与抓取结果验证

- **★ GG-CNN · RSS 2018** — *Closing the Loop for Robotic Grasping: A Real-time, Generative Grasp Synthesis Approach*. 从连续深度图更新抓取位姿，并测试抓取途中被移动的物体。重点看作者在什么时候移动目标、策略还能获得几次新观测。这会直接影响你的实验中第一次抓空的概率。[Paper](https://roboticsproceedings.org/rss14/p21.html) · [arXiv](https://arxiv.org/abs/1804.05172)
- **★ 部分遮挡下的抓取验证 · Frontiers in Neurorobotics 2021** — *Failure Handling of Robotic Pick and Place Tasks With Multimodal Cues Under Partial Object Occlusion*. Zhu 等结合视觉与软夹爪自身的感知信息处理抓放失败。设计“物体是否随夹爪抬起”判据时，优先看它如何处理夹爪遮挡。对只有相机的系统，可把“目标仍在桌上”和“目标暂时看不见”分开标注。[Paper](https://doi.org/10.3389/fnbot.2021.570507) · [全文](https://pmc.ncbi.nlm.nih.gov/articles/PMC7982538/)
- **The Feeling of Success · CoRL 2017** — *The Feeling of Success: Does Touch Sensing Help Predict Grasp Outcomes?* Calandra 等比较视觉与触觉对抓取结果预测的作用。适合参考其输入信号对照；迁移到视觉试抬方案时，还要独立定义“离桌并持稳”的评测真值。[Paper](https://www.robertocalandra.com/papers/2017_calandra_corl.pdf)
- **◇ Code-as-Monitor · CVPR 2025** — *Code-as-Monitor: Constraint-aware Visual Programming for Reactive and Proactive Robotic Failure Detection*. 将执行监测写成可由程序检查的时空约束。可借用这种表达方式，把成功条件拆成“夹爪已上升”“目标已离桌”“保持阶段二者相对位置稳定”，逐项检查误判来源。[Paper](https://arxiv.org/abs/2412.04455) · [Project](https://zhoues.github.io/Code-as-Monitor/)

## 失败后的重抓与动作修正

- **★ FIRE-Grasp · IEEE RA-L 2026** — *FIRE-Grasp: Failure-Informed Regrasping With Multimodal Evidence*. 与“失败后调整再抓”最接近。系统从首次失败中提取多模态证据，据此调整候选抓取排序和执行参数。重点看哪一种失败证据实际改变了第二次动作，再思考仅修正平面位置能够覆盖哪些失败。[Paper](https://doi.org/10.1109/LRA.2026.3706934)
- **★ ◇ FailSafe · arXiv 2025** — *FailSafe: Reasoning and Recovery from Failures in Vision-Language-Action Models*. 将失败样例与可执行恢复动作配对，在基础 VLA 执行时检测问题、介入纠错，再交还控制。可据此设计你的接口：检测器何时接管、平移多少、从哪个状态恢复原策略。[Paper](https://arxiv.org/abs/2510.01642) · [Project](https://jimntu.github.io/FailSafe/) · [Code](https://github.com/Jimntu/FailSafe_code)
- **More Than a Feeling · arXiv 2018** — *More Than a Feeling: Learning to Grasp and Regrasp using Vision and Touch*. 根据视觉、触觉以及候选调整动作预测下一次抓取的结果。适合理解“知道抓得不好”到“选哪一个动作”的中间环节，也可用于讨论仅凭目标中心偏差难以处理的接触问题。[Paper](https://arxiv.org/abs/1805.11085)
- **◇ RACER · ICRA 2025** — *RACER: Rich Language-Guided Failure Recovery Policies for Imitation Learning*. 由视觉语言模型观察执行过程、给出细粒度纠错指令，再由动作策略执行。若论文需要讨论 Agent 与 VLA 如何分工，可用它和 FailSafe 对照，比较语言指令与直接恢复动作两种接口。[Paper](https://arxiv.org/abs/2409.14674) · [Project](https://rich-language-failure-recovery.github.io/) · [Code](https://github.com/sled-group/RACER)
- **REFLECT · CoRL 2023** — *REFLECT: Summarizing Robot Experiences for Failure Explanation and Correction*. 利用多传感器执行历史生成失败解释，再指导规划器纠错，并提出 RoboFail 数据集。适合参考其时间线记录方法，区分“闭合时已经抓空”和“原本夹住、抬升后才滑落”。[Paper](https://proceedings.mlr.press/v229/liu23g.html) · [arXiv](https://arxiv.org/abs/2306.15724)

## VLA 执行监测与基础策略

- **◇ SAFE · arXiv 2025** — *SAFE: Multitask Failure Detection for Vision-Language-Action Models*. 从 VLA 内部特征估计失败风险。可与基于目标高度的外部检测器比较输入要求、报警时机和漏报情况；内部特征路线需要能访问模型表示。[Paper](https://arxiv.org/abs/2506.09937) · [Project](https://vla-safe.github.io/)
- **◇ AHA · ICLR 2025** — *AHA: A Vision-Language-Model for Detecting and Reasoning Over Failures in Robotic Manipulation*. 通过扰动成功示范构造失败轨迹，并训练模型解释操作失败。对本题最有用的是失败样本的构造与标注方式，可用于规划偏移方向、发生阶段和失败类别。[Paper](https://openreview.net/forum?id=JVkdSi7Ekg) · [Project](https://robofail-vlm.github.io/) · [Code](https://github.com/NVlabs/AHA)
- **◇ FAIL-Detect · RSS 2025** — *Can We Detect Failures Without Failure Data? Uncertainty-Aware Runtime Failure Detection for Imitation Learning Policies*. 研究缺少失败训练样本时如何在执行中报警。可与 SAFE 配对阅读，判断自己的检测器究竟需要多少失败样本做训练或阈值校准。[Paper](https://arxiv.org/abs/2503.08558) · [Project](https://cxu-tri.github.io/FAIL-Detect-Website/) · [Code](https://github.com/CXU-TRI/FAIL-Detect)
- **◇ OpenVLA · CoRL 2024** — *OpenVLA: An Open-Source Vision-Language-Action Model*. 三组实验可选的基础策略。优先核对检查点与仿真任务、相机输入和动作坐标系是否匹配；三组应保留同样的模型权重与视觉更新频率。[Paper](https://proceedings.mlr.press/v270/kim25c.html) · [Project](https://openvla.github.io/) · [Code](https://github.com/openvla/openvla)

## 重试基线与恢复评测

- **★ LIBERO-RECOVER · arXiv 2026** — *LIBERO-RECOVER: Beyond Task Success Towards Failure Recovery in Robotic Manipulation Models*. 基于真实策略执行中出现的失败构造恢复场景，其中明确区分 Action Retry 与 Action Adaptation。最适合用于论证为何应单独比较“再试一次”和“调整后再试”，并统计首次失败样本中的恢复率。[Paper](https://arxiv.org/abs/2609.05178)
- **Abort and Retry · IROS 2011** — *Abort and Retry in Grasping*. 根据抓取过程中的手部状态决定何时中止和重试，以完成一次成功抓取的期望时间评价策略。适合为时间指标提供依据，使回撤、重新观察与对齐产生的代价也能体现在结果中。[Paper](https://publications.ri.cmu.edu/abort-and-retry-in-grasping)

## 这些论文如何支撑三组实验

| 组别 | 建议的实验操作 | 优先参考 |
| --- | --- | --- |
| A · 原始 VLA | 保留原策略的视觉更新，记录一次抓取后的实际结果；按偏移幅度统计首次成功率。 | OpenVLA、GG-CNN |
| B · 失败后直接重试 | 从首次失败状态退至观察位，用最新图像再调用一次相同策略。记录恢复率与总耗时。 | LIBERO-RECOVER、Abort and Retry |
| C · 反馈调整后重试 | 与 B 共用首次轨迹、触发条件和最新观测。在再次调用策略前，显式估计目标相对夹爪的偏差并对齐。 | FIRE-Grasp、FailSafe、Code-as-Monitor |

比较 B、C 时，两组都能看到移动后的目标，也都只得到一次补抓机会。最有解释力的结果是同一批首次失败场景中，有多少只有 C 救回，又有多少只有 B 救回。另行报告试抬检测的漏报、误报和遮挡导致的无法判定，便能定位差异来自失败识别还是动作修正。
