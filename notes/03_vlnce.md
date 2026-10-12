# VLN-CE：Beyond the Nav-Graph

**论文**：Jacob Krantz 等，*Beyond the Nav-Graph: Vision-and-Language Navigation in Continuous Environments*，ECCV 2020。

- [论文 arXiv](https://arxiv.org/abs/2004.02857)
- [VLN-CE 项目页](https://jacobkrantz.github.io/vlnce/)

## 任务变化

传统 R2R 以离散全景节点和已知 connectivity graph 为基础，动作常常等价于“跳到下一个可见节点”。VLN-CE 把 agent 放回连续 3D 环境：只能看 egocentric RGB-D，执行低层动作，面对碰撞、遮挡、未知拓扑和不完美定位。

因此，模型必须同时解决：

- 从图像中感知可通行空间；
- 把语言里的地标和方向落到当前视野；
- 产生能够被连续控制器执行的目标；
- 在部分可观测历史中保持路线进度。

## 评测含义

常见指标包括 Navigation Error（NE）、Success Rate（SR）、SPL、nDTW/SDTW、轨迹长度和碰撞率。VLN-CE 的绝对分数通常低于离散 VLN，不能直接跨设置比较。

## 对 ETPNav 的影响

ETPNav 的在线 waypoint 与 ghost 图正好针对 VLN-CE 缺少已知导航图的问题。当前项目配置固定使用 Habitat 0.1.7 和 R2R-CE/RxR-CE 数据格式；评估时需要报告 split、场景、权重和动作设置，避免把离散结果误当作连续导航结果。

## 学习要点

VLN-CE 的难点不是单纯把动作类别变多，而是把高层语言规划与连续空间执行连接起来。任何新 waypoint 方法都要同时测：航点可达性、碰撞、闭环 SR/SPL，以及在未见场景中的泛化。

