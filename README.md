# Agent_learning
This repo is prepared for restoring some notes and demos.

## 模型采样参数
### Temperature
Definition: The prob distributions of the Top-K tokens depend on the set of temperature.

Low temperature : The weight of high prob module will increase, LLM tends to generate more conservative contents. That means the distribution is more steep.

High temperature : Generating multiple, emanative(innovative) answers.The distribution is more smooth.

高温度（0.7 ⩽ Temperature < 2）：输出 “创新、发散”。适用场景： 创意性任务：如诗歌创作、科幻故事构思、广告 slogan brainstorm、艺术灵感启发； 发散性思考。

### Top-K
Definition : Merge all of the tokens by prob. Choose the prior K token and normalization.

If topK decreases, the set of the tokens also decreases.When K decreases to 1, there is no effect of P and temperature.

Vice versa, when topK increases, there are more choices, but the accuracy decreases followingly.

(与温度采样的区别与联系：温度采样通过温度 T 调整所有 token 的概率分布（平滑或陡峭），不改变候选 token 的数量（仍考虑全部 N 个）。Top-k 采样通过 k 值限制候选 token 的数量（只保留前 k 个高概率 token），再从其中采样。当k=1时输出完全确定，退化为 “贪心采样”。)

### Top-P
Aggregate the prob of top-p tokens until it reaches the threshold. After that, normalization the conserved tokens.

与Top-k的区别与联系：相对于固定截断大小的 Top-k，Top-p 能动态适应不同分布的“长尾”特性，对概率分布不均匀的极端情况的适应性更好。

### 三者的联系
当三者同时存在时，其优先级顺序为：温度调整→Top-k→Top-p。

不过，通常 Top-k 和 Top-p 二选一即可，若同时设置，实际候选集为两者的交集。 

需要注意的是，如果将温度设置为 0，则 Top-k 和 Top-p 将变得无关紧要，因为最有可能的 Token 将成为下一个预测的 Token；

如果将 Top-k 设置为 1，温度和 Top-p 也将变得无关紧要，因为只有一个 Token 通过 Top-k 标准，它将是下一个预测的 Token。


## 从数学角度解读 上下文窗口 & 分词器

