# Sim-2-Sim：从离散 VLN 迁移到连续环境

**论文**：Jacob Krantz、Stefan Lee，*Sim-2-Sim Transfer for Vision-and-Language Navigation in Continuous Environments*，ECCV 2022。

- [论文 PDF](https://www.ecva.net/papers/eccv_2022/papers_ECCV/papers/136990580.pdf)
- [项目页](https://jacobkrantz.github.io/sim-2-sim)

## 思路

作者从离散环境中训练或获得高层 instruction follower，再加入子目标生成和连续低层导航的 software harness，把“在图上选节点”转成“在连续场景中走到目标”。论文报告这种迁移显著提高 VLN-CE 的成功率，并分析了每个迁移环节带来的性能损失。

## 与 ETPNav 的关系

两者都承认长程语言规划与连续运动控制是不同层次：

- Sim-2-Sim：尽量复用离散策略，外接连续执行 harness；
- ETPNav：在线构图，让 planner 直接在不断增长的 ghost 图上规划。

因此 ETP 更像“把迁移接口内化为在线拓扑图”，代价是需要可靠的 waypoint 预测和图维护。

## 对实验设计的启示

- 分别报告高层目标选择错误和低层执行错误；
- 固定同一 instruction、场景和初始位姿比较策略；
- 统计碰撞、卡住、重新规划和路径长度，不只看 SR。

