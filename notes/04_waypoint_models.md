# Waypoint Models：语言条件的连续航点预测

**论文**：Jacob Krantz 等，*Waypoint Models for Instruction-guided Navigation in Continuous Environments*，ICCV 2021。

- [论文 arXiv](https://arxiv.org/abs/2110.02207)
- [ICCV PDF](https://openaccess.thecvf.com/content/ICCV2021/papers/Krantz_Waypoint_Models_for_Instruction-Guided_Navigation_in_Continuous_Environments_ICCV_2021_paper.pdf)

## 核心问题

连续环境中，如果策略每一步只输出很小的低层动作，决策长度很长；如果直接输出很远的终点，又难以避障。Waypoint Models 研究中间层：让模型根据视觉和指令预测相对极坐标的 waypoint，再交给低层导航器执行。

论文比较了不同动作表达能力，观察到更有表达力的 waypoint 能产生更短、更容易执行的轨迹，但过于粗糙的目标可能牺牲近似最短路径的能力。

## 方法结构

```text
RGB-D 全景 + instruction
        -> cross-modal attention
        -> waypoint / STOP head
        -> 相对角度与距离
        -> 低层转向 + 前进
```

当前 ETP 的 `120 × 12` 角度—距离热力图可以看作这条路线的工程化、离散化版本；ETP 再额外增加候选航点、拓扑图和全局 planner。

## 对当前研究的启发

- 航点的 Top-K 召回比只看 Top-1 更重要，因为全局 planner 需要保留恢复路径。
- 候选目标必须通过实际控制验证，NavMesh 上的点不等于控制器一定能到达。
- 若加入语言条件分支，必须做指令打乱、置空和同场景多目标实验，证明模型确实读取语言。

