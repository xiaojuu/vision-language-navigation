# Uni-NaVid：统一具身导航任务的视频 VLA

**论文**：Jiazhao Zhang 等，*Uni-NaVid: A Video-based Vision-Language-Action Model for Unifying Embodied Navigation Tasks*，arXiv:2412.06224 / RSS 2025。

- [论文 arXiv](https://arxiv.org/abs/2412.06224)
- [项目页](https://pku-epic.github.io/Uni-NaVid/)

## 核心思想

Uni-NaVid 不为 instruction following、object navigation、embodied QA、human following 分别训练模型，而是统一输入/输出协议，用单个视频 VLA 处理多种导航任务。模型接收自然语言和单目 RGB 视频，端到端生成低层动作或语言响应。

长视频的主要工程难点是 token 数量和延迟。论文使用 online visual token merge，在空间和时间上合并相似视觉信息，以支持更长历史和更高频率的实际执行。

## 与 ETPNav 的对比

| 维度 | ETPNav | Uni-NaVid |
|---|---|---|
| 规划接口 | waypoint + 在线拓扑图 | 端到端 VLA action chunks |
| 几何输入 | RGB-D，显式可达性 | 主要依靠视频表征 |
| 历史 | recurrent/cross-modal planner + graph | 视频 token 压缩 |
| 优点 | 可解释、易回退、适配 VLN-CE | 任务统一、接口简洁、适合真实机器人 |
| 风险 | waypoint/图误差传播 | 长程几何和可执行性隐式化 |

## 对本项目的启发

如果未来接入大视觉语言模型，token merge/历史压缩可以作为效率模块，但不能让它取代 ETP 的 waypoint 可达性检查。更实际的路线是让大模型提供高层语义或进度状态，保留 ETP 负责连续控制和安全回退。

