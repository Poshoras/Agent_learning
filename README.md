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
### 上下文窗口
假设上下文窗口是8000，在输入一个问题的时候，同时涉及系统提示词，历史对话，RAG检索内容，以及tool生成的内容，这些都会消耗token，因此要控制输入问题的质量，否则会超过token限制。
### 分词器
这个消耗token的数量，显然和分词器的构成是有关系的，BERT和BPE的消耗是不同的。



## 模型选型的关键考量

在为您的智能体选择大语言模型时，可以从以下几个维度进行综合评估：

|A | Description|
|-------|-----|
|性能与能力|这是最核心的考量。不同的模型擅长的任务不同，有的长于逻辑推理和代码生成，有的则在创意写作或多语言翻译上更胜一筹。您可以参考一些公开的基准测试排行榜（如 LMSys Chatbot Arena Leaderboard）来评估模型的综合能力。|
|成本|对于闭源模型，成本主要体现在 API 调用费用，通常按 Token 数量计费。对于开源模型，成本则体现在本地部署所需的硬件（GPU、内存）和运维上。需要根据应用的预期使用量和预算做出选择。|
|速度（延迟)|对于需要实时交互的智能体（如客服、游戏 NPC），模型的响应速度至关重要。一些轻量级或经过优化的模型（如 GPT-3.5 Turbo, Claude 3.5 Sonnet）在延迟上表现更优。|
|上下文窗口|模型能一次性处理的 Token 数量上限。对于需要理解长文档、分析代码库或维持长期对话记忆的智能体，选择一个拥有较大上下文窗口（如 128K Token 或更高）的模型是必要的|
|部署方式|使用 API 的方式最简单便捷，但数据需要发送给第三方，且受限于服务商的条款。本地部署则能确保数据隐私和最高程度的自主可控，但对技术和硬件要求更高。|
|生态与工具链|一个模型的流行程度也决定了其周边生态的成熟度。主流模型通常拥有更丰富的社区支持、教程、预训练模型、微调工具和兼容的开发框架（如 LangChain, LlamaIndex, Hugging Face Transformers），这能极大地加速开发进程，降低开发难度。选择一个拥有活跃社区和完善工具链的模型，可以在遇到问题时更容易找到解决方案和资源。|
|可微调性与定制化|对于需要处理特定领域数据或执行特定任务的智能体，模型的微调能力至关重要。一些模型提供了便捷的微调接口和工具，允许开发者使用自己的数据集对模型进行定制化训练，从而显著提升模型在特定场景下的性能和准确性。开源模型在这方面通常提供更大的灵活性。|
|安全性与伦理|随着大语言模型的广泛应用，其潜在的安全风险和伦理问题也日益凸显。选择模型时，需要考虑其在偏见、毒性、幻觉等方面的表现，以及服务商或开源社区在模型安全和负责任AI方面的投入。对于面向公众或涉及敏感信息的应用，模型的安全性和伦理合规性是不可忽视的考量。|
