# DUET：Think Global, Act Local

**论文**：Shizhe Chen 等，*Think Global, Act Local: Dual-scale Graph Transformer for Vision-and-Language Navigation*，CVPR 2022。

- [论文 arXiv](https://arxiv.org/abs/2202.11742)
- [项目页](https://cshizhe.github.io/projects/vln_duet.html)
- [官方代码](https://github.com/cshizhe/VLN-DUET)

## 方法

DUET 在线构建已访问 viewpoint 的拓扑图，并用 dual-scale graph transformer 分成两条互补路径：

- **Fine scale**：当前节点和局部可见方向，精细做语言—视觉 grounding；
- **Coarse/global scale**：整个在线地图，支持跳到远期节点的长程动作规划。

两种尺度动态融合，使模型同时保留“当前应该怎么转”和“长期应该去哪儿”。

## 与 ETPNav 的关系

ETP 的 cross-modal planner、在线 ghost 图和预训练脚本明显处在同一技术脉络。理解 DUET 有助于区分：

- 图结构解决长程搜索和记忆；
- 局部视觉 token 解决当前动作 grounding；
- waypoint/ghost 只是把连续世界转成可规划的候选图。

## 对当前项目的启发

SC-WP 不应只把子指令喂给局部预测器；它还要考虑全局 planner 当前选择的 ghost 和路线进度。UA-ETP 的 uncertainty embedding 也应同时作用于局部候选与全局节点风险。

