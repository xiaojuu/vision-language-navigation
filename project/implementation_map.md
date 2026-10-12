# 论文方法到代码的映射

## ETPNav 基线

- `vlnce_baselines/waypoint_pred/TRM_net.py`：深度驱动的 waypoint predictor。
- `vlnce_baselines/waypoint_pred/transformer/waypoint_bert.py`：航点热力图的 Transformer 主干。
- `vlnce_baselines/models/Policy_ViewSelection_ETP.py`：预测热力图、NMS、候选 waypoint、ETP policy 接口。
- `vlnce_baselines/models/graph_utils.py`：在线图/ghost 节点维护。
- `vlnce_baselines/models/etp/vilmodel_cmt.py`：跨模态全局 planner。
- `vlnce_baselines/ss_trainer_ETP.py`：训练、验证、推理和 waypoint predictor 加载。
- `vlnce_baselines/common/base_il_trainer.py`：Habitat rollout 与候选航点调用。

## 语义 waypoint MVP

- `vlnce_baselines/semantic_waypoint/dataset.py`：JSONL manifest、RGB-D、离散 GT、可选原始 ETP logits。
- `vlnce_baselines/semantic_waypoint/fusion.py`：零初始化的 text-conditioned residual fusion 和 loss。
- `tools/export_semantic_waypoint_dataset.py`：导出局部当前点—目标点样本。
- `tools/cache_waypoint_logits.py`：缓存原始 ETP `120×12` logits。
- `tools/train_semantic_waypoint_probe.py`：单场景离线残差探针。
- `tools/run_grouped_semantic_probe.py`：按轨迹分组的多种子验证。
- `tools/analyze_waypoint_alignment.py`：量化、预测峰值、NavMesh 和可视化检查。

## 复现实验顺序

```text
1. 先运行官方 ETP checkpoint 的 val-seen/val-unseen baseline
2. 检查 waypoint heatmap、Top-K 覆盖率和控制器可达性
3. 运行 semantic waypoint dataset / probe
4. 完成正确文本、置空文本、错误文本和文本打乱对照
5. 再把 SC-WP 接回闭环 planner
6. 最后逐项加入 UA-ETP 的置信度融合、关联和风险头
```

## StageVLN 风格扩展的最小实现

1. 在训练循环中暴露 ETP planner 的多层 hidden states；
2. 离线运行几何教师，缓存与每步观测对齐的 teacher features；
3. 增加多层 projector、relative heading head、expert progress head；
4. 训练时加入辅助损失，验证/推理时不调用教师和辅助头；
5. 保留 `SFT / trajectory-only / geometry-only / full` 四组消融；
6. 检查辅助监督没有读取目标图像、GT waypoint 或测试时不可得的真实位姿。

