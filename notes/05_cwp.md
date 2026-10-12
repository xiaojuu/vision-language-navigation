# CWP：弥合离散与连续 VLN 的差距

**论文**：Yicong Hong 等，*Bridging the Gap Between Learning in Discrete and Continuous Environments for Vision-and-Language Navigation*，CVPR 2022。

- [CVPR 论文页](https://openaccess.thecvf.com/content/CVPR2022/html/Hong_Bridging_the_Gap_Between_Learning_in_Discrete_and_Continuous_Environments_CVPR_2022_paper.html)
- [官方代码](https://github.com/YicongHong/Discrete-Continuous-VLN)

## 关键观察

离散 VLN 已知 connectivity graph，模型可以对每个可见方向直接分类；连续 VLN 没有这个图，必须先从 RGB-D 中找出可达位置。CWP（candidate waypoint predictor）把这个接口显式化：预测局部可达候选，再让离散式导航策略在候选上规划。

## 为什么对 ETP 重要

ETP 的 waypoint predictor 延续了 CWP 的“先提出可达候选，再做高层选择”思路，同时把候选在线组织成拓扑图。当前代码里的深度编码器、角度—距离热力图、NMS 和可执行候选，都应放在 CWP 的连续化视角下理解。

## 复现时要分清三种误差

1. **量化误差**：GT 相对角度/距离落到离散 bin 后的误差；
2. **预测误差**：热力图峰值与 GT bin 的差异；
3. **执行误差**：控制器执行预测航点后是否碰撞或偏离。

它们不能合并为一个“航点准确率”。当前项目 `tools/analyze_waypoint_alignment.py` 正是为了拆开这三类误差。

## 对 SC-WP 的启发

语言条件化不能牺牲几何可达候选的召回率。较稳妥的做法是保留语义候选和几何保底候选，分别统计文本敏感性和 Top-K 可达覆盖率。

