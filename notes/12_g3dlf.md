# g3D-LF：可泛化的 3D-语言特征场

**论文**：Zihan Wang、Gim Hee Lee，*g3D-LF: Generalizable 3D-Language Feature Fields for Embodied Tasks*，CVPR 2025。

- [论文 arXiv](https://arxiv.org/abs/2411.17030)
- [CVPR PDF](https://openaccess.thecvf.com/content/CVPR2025/papers/Wang_g3D-LF_Generalizable_3D-Language_Feature_Fields_for_Embodied_Tasks_CVPR_2025_paper.pdf)

## 核心思想

g3D-LF 用带位姿的 RGB-D 图像构建可动态更新的 3D-language feature field，并从任意新视角预测：

- novel-view 的视觉表示；
- 以 agent 为中心的 BEV map；
- 多粒度语言查询结果。

通过体渲染潜在特征、多尺度编码和多层对比学习，模型把空间和语言对齐，而不是只在离散视角上做匹配。

## 与 ETP/StageVLN 的关系

- 与 ETP：g3D-LF 可作为更强的离线空间教师或语义地图来源；但它的 3D field 不应直接成为推理依赖，否则会增加显存和延迟。
- 与 StageVLN：它提供了“空间教师”的一种候选实现；StageVLN 更关注把教师知识蒸馏进导航器，推理时移除教师。
- 与 BEV-Bert：都强调以地图/鸟瞰结构减少视角重复和空间歧义；g3D-LF 的表征更连续、可从新视角查询。

## 工程风险

g3D-LF 需要可靠的 RGB-D 与位姿，训练数据规模和预训练成本较高。当前 ETP 项目只有局部观测对和少量场景验证，不能直接宣称已经具备训练 g3D-LF 的数据条件。

