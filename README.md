🌍 TravelForge: 多智能体 (Multi-Agent) 协同规划与自动执行引擎

TravelForge 是一个面向复杂出行场景的智能 Agent 工作流系统。系统基于 LangChain 架构，整合了 RAG（检索增强生成）与多智能体（Multi-Agent）协同处理能力，旨在解决传统出行规划中“信息碎片化、预算不可控、执行难落地”的核心痛点。

本项目在底层架构上参考了优秀的开源 Agent 框架，并在此基础上进行了深度的全局状态流转重构与多智能体容错优化，具备极高的商业落地参考价值。

🚀 核心架构优化与个人贡献 (✨ Highlight)

相比于基础的演示级开源框架，本项目在工程化落地上进行了以下深度优化：

1.重构全局状态管理 (Two-way Data Binding)

  痛点：传统 Streamlit AI 项目中，前端静态表单与后端 Agent 对话历史存在严重的“状态割裂”。

  重构方案：利用 st.session_state 引入双向数据绑定机制，打通了用户自然语言输入与结构化表单之间的壁垒，实现复杂交互场景下的状态实时同步。

2.多 Agent 协同与 Plan-and-Solve 容错调度

  采用 Planner（规划器）与 Executor（执行器）分离的架构。

  针对长文本字符串骤发导致的模型格式波动问题进行自动规范化，大幅提升系统对模糊约束（如交通方式、预算未指定）的边界处理能力和防崩溃能力。

3.基于 RAG 的防幻觉机制

  挂载本地 BAAI/bge-small-zh-v1.5 向量模型与 Chroma 持久化知识库。

  确保景点开放时间、酒店历史报价等信息的客观真实性，通过底层 Prompt 强约束大语言模型的发散性幻觉。

🛠️ 当前核心能力矩阵 (Features)

 🧠 自然语言参数抽取：多轮对话记忆，自动维护和更新结构化出行需求状态（目的地、天数、人数、预算等）。

 🔗 外部工具调用 (MCP 集成)：

  高德地图 MCP：实时查询天气、POI（兴趣点）精准位置。

  酒店 MCP：接入酒店搜索与价格查询 API，支持 ModelScope 代理机制。

 💰 动态预算估算：结合城市消费系数与酒店 MCP 中位参考价，进行安全基础数学计算。

 🔄 Reflection (反思迭代)：内置最多 2 轮结果质检与自动优化，确保最终输出的路线合理性与预算可行性。

 📝 分层产出：支持“草案/正式”分层，粗略规划跳过 Reflection 直接输出，正式报告则进入完整质检并支持 Markdown 一键导出。

⚙️ 技术栈 (Tech Stack)

| 模块 | 技术方案 |
| :--- | :--- |
| **底层框架** | LangChain / Python 3.10 |
| **大语言模型 (LLM)** | DeepSeek (主逻辑调度与反思) / 豆包 (轻量级抽取与意图理解) |
| **RAG 向量检索** | ChromaDB / `BAAI/bge-small-zh-v1.5` |
| **前端交互** | Streamlit |
| **工具调用** | 官方 MCP (Model Context Protocol) 标准规范 |

💻 快速开始 (Quick Start)

1. 环境准备

推荐使用 Conda 创建隔离的虚拟环境：

conda create -n agent python=3.10 -y
conda activate agent
pip install -r requirements.txt
pip install langchain-deepseek


2. 环境变量配置

复制项目根目录下的 .env.example 文件并重命名为 .env，填入以下必要参数：

api_key=your_deepseek_api_key
base_url=[https://api.deepseek.com](https://api.deepseek.com)
model_id=deepseek-chat
# 其他 MCP 与本地向量库路径请参考文档默认配置


3. 构建本地 RAG 知识库

首次运行前必须构建本地向量数据库：

python -m travel_forge.rag.build_index


4. 启动 Web 服务

python -m travel_forge.app.run_ui


运行后，浏览器将自动打开 http://localhost:8501。

📂 项目结构概览

TravelForge/
├── travel_forge/            # 核心业务代码逻辑
│   ├── agents/              # 多智能体定义 (Planner, Executor, Reflector)
│   ├── rag/                 # 检索增强生成模块 (向量化、索引构建)
│   ├── tools/               # 外部工具封装 (高德 MCP, 酒店搜索, 预算计算)
│   ├── app/                 # Streamlit UI 与入口端
│   ├── memory.py            # 上下文多轮对话记忆管理
│   └── mcp_client.py        # MCP 协议客户端
├── data/                    # 本地 RAG 种子文档与 Chroma 数据库
├── docs/                    # 开发与系统架构文档
├── README.md                
├── requirements.txt         # 依赖清单
└── .env                     # (未提交) 环境与密钥配置


📅 未来规划 (Roadmap)

 [ ] 引入 Redis 缓存机制，优化相同目的地的二次检索速度。

 [ ] 接入多模态模型，支持根据游记图片反向生成路线规划。

 [ ] 开发用户登录模块，实现个人历史行程的云端持久化存储。

