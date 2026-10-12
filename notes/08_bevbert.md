# BEV-Bert：多模态地图预训练

**论文**：Dong An 等，*BEVBert: Multimodal Map Pre-training for Language-guided Navigation*，ICCV 2023。

- [论文 arXiv](https://arxiv.org/abs/2212.04385)
- [官方代码](https://github.com/MarSaKi/VLN-BEVBert)

## 核心贡献

只在离散 panorama token 上预训练，会让模型自己解决重复视角、遮挡和空间关系。BEV-Bert 用混合地图显式组织信息：

- **metric map**：局部鸟瞰/度量空间，聚合不完整观测并去重；
- **topological map**：保留长期规划所需的节点和关系。

在这种 topo-metric map 上做多模态预训练，使语言与空间位置、地图结构和导航目标对齐。

## 与 ETP 的关系

ETP 的 ghost 图偏向在线可执行规划；BEV-Bert 更强调地图表示的预训练。二者共同说明“把空间结构显式化”比让纯视觉 token 自己推断几何更稳定，但地图形式不同：ETP 追求轻量、在线和连续控制接口，BEV-Bert 追求空间感知的跨模态预训练。

## 对研究的启发

如果给 ETP 增加 StageVLN 风格几何教师，教师监督应优先落到地图/节点相关隐藏状态，而不是把完整点云直接输入推理器；否则会破坏 ETP 的部署路径。

