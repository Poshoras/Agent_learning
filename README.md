# Agent_learning_basic_knowledge
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

|维度 | Description|
|-------|-----|
|性能与能力|这是最核心的考量。不同的模型擅长的任务不同，有的长于逻辑推理和代码生成，有的则在创意写作或多语言翻译上更胜一筹。您可以参考一些公开的基准测试排行榜（如 LMSys Chatbot Arena Leaderboard）来评估模型的综合能力。|
|成本|对于闭源模型，成本主要体现在 API 调用费用，通常按 Token 数量计费。对于开源模型，成本则体现在本地部署所需的硬件（GPU、内存）和运维上。需要根据应用的预期使用量和预算做出选择。|
|速度（延迟)|对于需要实时交互的智能体（如客服、游戏 NPC），模型的响应速度至关重要。一些轻量级或经过优化的模型（如 GPT-3.5 Turbo, Claude 3.5 Sonnet）在延迟上表现更优。|
|上下文窗口|模型能一次性处理的 Token 数量上限。对于需要理解长文档、分析代码库或维持长期对话记忆的智能体，选择一个拥有较大上下文窗口（如 128K Token 或更高）的模型是必要的|
|部署方式|使用 API 的方式最简单便捷，但数据需要发送给第三方，且受限于服务商的条款。本地部署则能确保数据隐私和最高程度的自主可控，但对技术和硬件要求更高。|
|生态与工具链|一个模型的流行程度也决定了其周边生态的成熟度。主流模型通常拥有更丰富的社区支持、教程、预训练模型、微调工具和兼容的开发框架（如 LangChain, LlamaIndex, Hugging Face Transformers），这能极大地加速开发进程，降低开发难度。选择一个拥有活跃社区和完善工具链的模型，可以在遇到问题时更容易找到解决方案和资源。|
|可微调性与定制化|对于需要处理特定领域数据或执行特定任务的智能体，模型的微调能力至关重要。一些模型提供了便捷的微调接口和工具，允许开发者使用自己的数据集对模型进行定制化训练，从而显著提升模型在特定场景下的性能和准确性。开源模型在这方面通常提供更大的灵活性。|
|安全性与伦理|随着大语言模型的广泛应用，其潜在的安全风险和伦理问题也日益凸显。选择模型时，需要考虑其在偏见、毒性、幻觉等方面的表现，以及服务商或开源社区在模型安全和负责任AI方面的投入。对于面向公众或涉及敏感信息的应用，模型的安全性和伦理合规性是不可忽视的考量。|


# Agent_build


## 环境配置

首先记录一下终端修改路径的命令，这是最基础的：

|移动路径| 指令|
|-------|-----|
|最复杂的进入文件夹的路径：|cd /Users/isrealpeacelee/Desktop/AI/Agent1|
|简化版本: |cd "AI/Agent1"|
|退回桌面：|cd ~/Desktop|
|从AI到Agent1：|cd Agent1|
|从Agent1退到AI：|cd ..|
|从Agent1退回桌面：| cd ../..|






| 步骤 | 操作 | 命令 / 内容 | 说明 |
|------|------|-------------|------|
| 1 | 打开项目 | 在 VS Code 中打开项目文件夹 | 确保 Terminal 位于项目根目录 |
| 2 | 打开 Terminal | `Terminal → New Terminal` | 打开终端 |
| 3 | 确认项目路径 | `pwd` | 确认当前位于项目根目录 |
| 4 | 创建 `.env` | `touch .env` | 创建环境变量配置文件 |
| 5 | 检查文件 | `ls -la` | 确认 `.env` 创建成功 |
| 6 | 配置 API | `open -e .env` | 打开 `.env` 并填写 API 配置 |
| 7 | 填写配置 | `MODEL_ID=deepseek-chat` | 设置模型 ID |
|  |  | `API_KEY=YOUR_DEEPSEEK_API_KEY` | 设置 DeepSeek API Key |
|  |  | `BASE_URL=https://api.deepseek.com` | 设置 API 服务地址 |
| 8 | 安装依赖 | `pip install python-dotenv` | 安装环境变量读取工具 |
| 9 | 配置 Git | 在 `.gitignore` 中加入 `.env` | 防止 API Key 被上传到 GitHub |

command + shift + . 会显示env文件（正常情况下会被隐藏，无法通过点击文件的方式打开）


写代码的时候要有环境意识！！

                    你的电脑
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       Agent项目    ML项目       其他项目
          │            │            │
       agent环境    ml环境      xxx环境
          │            │            │
     Python 3.12   Python 3.11   Python ...


现在已经创建好了一个agent的环境。然后以后可以选择那个（agent）开头的环境。

新安装库：

打开Vscode的terminal，然后输入：conda activate agent，之后 指令 python -m pip install ......

**由于在同一个路径下的ipynb文件会共用一个kernel，即使你把其中一个ipynb改成py然后把它删了，之前的数据仍然留在kernel中，这种情况下会不停的报错until重启kernel**


## 概念梳理
React，Plan and solve，reflection这叫做思考模式

Langgraph这种叫做框架，框架是你要按照这个规定去写代码。Langgraph可以采用react也可以采用reflection的思考模式。

这个Hello agent的课程，就是自己在搭建一个agent框架，而不是采用的任何外部封装好的框架（不是langchain，langgraph，AutoGen）这些框架。

那框架这种概念是如何体现出来的呢？ 和我自己写的时候区别在哪里呢？ 优势在哪里呢？ React这种思考模式又是如何被镶嵌在框架中呢？晚上先搞懂langgraph框架，然后去思考这几个问题。
