# Vision-Language Navigation 论文学习笔记

这是一份从 `ETPNav-main` 项目代码、实验记录和相关原论文整理出的中文学习结果，目标是把“读过什么、理解了什么、如何映射到代码、哪些结论已经被本项目验证”放在一个可持续更新的仓库中。

## 学习范围

项目原有 README 明确记录了 ETPNav、waypoint、BERT/VLN-BERT、CLIP、Uni-NaVid、BEV-Bert、DUET 和 g3D-LF；本次补充了它们的原论文笔记，并加入用户指定的 StageVLN。VLN-CE 任务论文、Sim-2-Sim 和 CWP/waypoint 工作作为 ETPNav 的直接技术脉络一并整理。

| 笔记 | 关键词 | 与当前项目的关系 |
|---|---|---|
| [StageVLN](notes/01_stagevln.md) | 训练期空间蒸馏、航向、轨迹进度 | 研究启发：把特权几何监督放到训练期，推理保持轻量 |
| [ETPNav](notes/02_etpnav.md) | 在线拓扑图、ghost 节点、全局规划、低层控制 | 当前代码基线 |
| [VLN-CE](notes/03_vlnce.md) | 连续环境、低层动作、评测假设 | 当前 Habitat 任务设定 |
| [Waypoint Models](notes/04_waypoint_models.md) | 语言条件航点、连续动作空间 | 当前 waypoint predictor 的方法来源 |
| [CWP](notes/05_cwp.md) | 离散到连续、候选航点预测 | ETP 航点模块的直接前置工作 |
| [Sim-2-Sim](notes/06_sim2sim.md) | 离散策略迁移、子目标生成、连续执行 | ETP 训练/执行分层的对照脉络 |
| [DUET](notes/07_duet.md) | dual-scale graph transformer、在线拓扑图 | ETP 全局规划器与预训练的重要来源 |
| [BEV-Bert](notes/08_bevbert.md) | topo-metric map、多模态地图预训练 | 解释地图表示和空间语言对齐 |
| [VLN-BERT](notes/09_vlnbert.md) | recurrent state、跨模态历史记忆 | 当前 `vlnce_baselines/models/etp/vilmodel_cmt.py` 的背景 |
| [BERT 与 CLIP](notes/10_bert_clip.md) | MLM、视觉语言对齐 | 文本编码器和视觉语义表征基础 |
| [Uni-NaVid](notes/11_uninavid.md) | 视频 VLA、统一导航任务、token merge | 现代端到端连续导航的对照方法 |
| [g3D-LF](notes/12_g3dlf.md) | 3D-language feature field、novel view、BEV | 几何/语言联合表征的前沿方向 |

## 从论文到本项目的一条主线

```text
VLN-CE 连续任务
  -> CWP/Waypoint：预测可执行局部航点
  -> ETPNav：把航点组织成在线拓扑图并做长程规划
  -> DUET/BEV-Bert：用地图表示支持全局语言 grounding
  -> VLN-BERT/BERT/CLIP：提供跨模态和历史状态编码
  -> 本项目 SC-WP + UA-ETP：让局部航点读取子指令，并传播不确定性
  -> StageVLN：进一步探索训练期特权空间/轨迹监督，推理端不增加几何模型
```

## 本项目已经得到的证据

- ETP 原始 waypoint predictor 输出 `120 × 12` 的角度—距离热力图，候选航点再进入在线拓扑规划。
- 语义航点 MVP 已导出训练集 471 条语言样本、157 组去重观测对；验证集为 15 条语言样本、5 组观测对。
- Qwen3-VL 子指令分解在训练 75/75、验证 3/3 条独立指令上成功解析，但活动子指令仍未可靠对齐到每个时间步。
- 分组探针显示文本会影响航点残差，但收益较小且跨随机种子不稳定；当前不能宣称语义航点已经提升 ETP 导航成功率。
- 位置扰动验证已经覆盖 1 个场景、3 条路线、18 个样本，但尚未执行参数更新或完整预训练适配。

详细证据见 [`project/evidence.md`](project/evidence.md)，实现入口见 [`project/implementation_map.md`](project/implementation_map.md)。

## 阅读和复现约定

1. 论文笔记中的“论文结论”只根据原论文摘要、正文和官方代码说明；“本项目观察”只根据仓库内实际文档、日志和脚本。
2. 不把离线 waypoint 探针的端点误差当作 SR/SPL；不把单场景结果推广成泛化结论。
3. StageVLN 的数值是论文作者报告的结果，尚未在本仓库复现。
4. 大数据集、模型权重和 Matterport3D 场景不随笔记仓库上传，只保留获取地址和复现实验命令。

## 目录

- `notes/`：逐篇论文的中文学习笔记。
- `project/`：ETPNav-main 中的实验事实、代码映射和后续研究计划。
- `bibliography.md`：统一文献入口和引用信息。

更新时间：2026-10-12（Asia/Shanghai）

## 历史学习记录（原 README 内容保留）

早期记录包含：ETPNav、waypoint、BERT/VLN-BERT、CLIP、Uni-NaVid、BEV-Bert、DUET 和 g3D-LF。相关图片仍保留在原 GitHub README 的历史版本中；本次整理把这些条目扩展成了 `notes/` 下的逐篇笔记。

