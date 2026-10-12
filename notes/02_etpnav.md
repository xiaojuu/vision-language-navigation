# ETPNav：连续环境中的演化拓扑规划

**论文**：Dong An 等，*ETPNav: Evolving Topological Planning for Vision-Language Navigation in Continuous Environments*，arXiv:2304.03047（TPAMI 2024）。

- 原文：[arXiv](https://arxiv.org/abs/2304.03047)
- 官方实现：[MarSaKi/ETPNav](https://github.com/MarSaKi/ETPNav)

## 解决什么问题

VLN-CE 要求智能体在连续 3D 场景中执行低层动作。只输出 `forward/left/right` 会让长程规划困难；只在离散全景图节点上规划又依赖已知导航图。ETPNav 的核心分工是：

```text
局部感知 -> waypoint predictor -> 在线拓扑图 -> 跨模态全局规划 -> 低层避障控制
```

它在运行中用预测航点自组织出 ghost 节点，不预先读取完整环境拓扑。

## 核心机制

### 1. Candidate waypoint prediction

输入当前 RGB-D 全景，预测角度—距离热力图。当前项目中的原始预测器输出 `120 × 12`：120 个角度 bin（每个约 3°）和 12 个距离 bin（步长 0.25m，最大 3m）。对热力图做 NMS 后得到少量候选航点；它们是“可走到的局部目标”，不是最终动作序列。

### 2. Evolving topological map

每个候选航点被转换到 agent 坐标系/世界坐标系，作为 ghost 节点加入图。新观测到的节点与历史节点按距离和可见关系连接，当前节点特征还会被更新。这样地图只保存导航决策需要的结构，不必重建完整稠密几何地图。

### 3. Cross-modal global planning

Transformer planner 同时编码指令和拓扑节点特征，在图上选择远期目标 ghost 或 STOP。全局规划器负责“去哪一个节点”，低层控制器负责“怎样绕开障碍抵达该节点”。

### 4. Trial-and-error controller

控制器先按目标方向旋转并前进；若碰撞、卡住或目标方向变化异常，则调整动作尝试并重新规划。这是连续环境中重要的执行保险丝。

## 论文启示

- 长程规划和低层控制要解耦，但两者的接口必须是可执行的局部目标。
- 在线拓扑图比纯端到端动作预测更容易解释、回退和重新规划。
- waypoint 预测误差会沿着“候选 -> ghost -> 规划”链路放大，因此置信度、节点关联和错误合并是后续研究的关键。

## 当前仓库对应关系

| 论文组件 | 代码位置 |
|---|---|
| waypoint predictor | `vlnce_baselines/waypoint_pred/TRM_net.py` |
| waypoint 候选/NMS/执行接口 | `vlnce_baselines/models/Policy_ViewSelection_ETP.py` |
| 拓扑图与 ghost | `vlnce_baselines/models/graph_utils.py` |
| 全局 planner | `vlnce_baselines/models/etp/vilmodel_cmt.py` |
| 训练、评估、推理循环 | `vlnce_baselines/ss_trainer_ETP.py`、`vlnce_baselines/common/base_il_trainer.py` |
| 运行入口 | `run.py`、`run_r2r/main.bash`、`run_rxr/main.bash` |

## 局限与本项目研究方向

原始 ETP 将 waypoint 当作确定性候选，并倾向用固定距离阈值和等权更新融合 ghost。当前项目的研究计划因此提出两步扩展：

1. **SC-WP**：让子指令、RGB 和 depth 共同指导局部航点，同时保留几何候选作为安全回退；
2. **UA-ETP**：把热力图不确定性传播到节点关联、位置融合和风险规划。

这两个方向是基于 ETP 误差传播链的研究假设，不能把计划表里的空指标当成已发表结果。

