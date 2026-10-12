# BERT 与 CLIP：项目中的两个基础表征来源

## BERT

**论文**：Jacob Devlin 等，*BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding*，2018。

- [原论文](https://arxiv.org/abs/1810.04805)

BERT 用 Transformer encoder 和 masked language modeling（MLM）学习双向上下文，再用很小的任务头微调。ETP 的 instruction encoder、tokenizer、MLM/SAP 预训练配置都可从这个范式理解：先学语言/跨模态表示，再针对导航动作微调。

工程注意：tokenizer、词表、最大长度、special token 和 checkpoint 配置必须成套使用；只替换编码器而不匹配 tokenizer 会造成静默的文本错位。

## CLIP

**论文**：Alec Radford 等，*Learning Transferable Visual Models From Natural Language Supervision*，2021。

- [原论文](https://arxiv.org/abs/2103.00020)
- [官方代码](https://github.com/openai/CLIP)

CLIP 用大规模 image-text pairs 的对比学习把图像和文本映射到同一空间。它适合做开放词汇语义检索和零样本匹配，但它的训练目标不是连续导航的可执行性，也不提供 waypoint 几何或历史进度。

## 对当前语义航点实验的启发

文本特征可以作为残差条件，修正 ETP 的几何 heatmap；但 CLIP/BERT 语义相似度本身不能替代 NavMesh 可达性、碰撞验证和在线图规划。语义分支必须保留几何候选回退，并用错误指令/文本置空实验确认模型没有只靠场景先验。

