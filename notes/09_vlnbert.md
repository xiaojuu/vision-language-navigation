# VLN-BERT：带循环状态的视觉语言 BERT

**论文**：Yicong Hong 等，*A Recurrent Vision-and-Language BERT for Navigation*，CVPR 2021。

- [论文 arXiv](https://arxiv.org/abs/2011.13922)
- [CVPR 论文页](https://openaccess.thecvf.com/content/CVPR2021/html/Hong_VLN_BERT_A_Recurrent_Vision-and-Language_BERT_for_Navigation_CVPR_2021_paper.html)
- [官方代码](https://github.com/YicongHong/Recurrent-VLN-BERT)

## 解决的问题

VLN 是部分可观测序列决策，单帧 cross-modal encoder 不足以表示历史。VLN-BERT 在 BERT 主干中加入 recurrent state token，跨时间传递跨模态状态，而不是另外堆一个 LSTM 或显式 memory buffer。

为减少开销，语言 token 主要作为 key/value，当前视觉和状态 token attend 到语言；这保持了 instruction grounding，同时降低全量双向注意力的成本。

## 与当前代码的关系

ETP 的 `vlnbert`/CMT 模块承担类似的历史视觉—语言融合职责。理解 recurrent state 有助于实现子指令进度选择：当前子指令不应只根据单帧决定，还要读取历史状态并施加单调约束。

## 学习结论

- 长指令执行需要记忆“已经完成了什么”；
- 预训练语言/视觉知识能明显改善收敛；
- 把语言 token 固定为 K/V 是一种有实际显存收益的跨模态设计；
- 若改变 state token 的 shape 或位置编码，必须重新检查 checkpoint 兼容性。

