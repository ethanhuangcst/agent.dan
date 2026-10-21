# 记忆与上下文类模式：Memory Management、Learning and Adaptation、Goal Setting、RAG

来源：《Agentic Design Patterns》第 8、9、11、14 章。这四个模式解决 agent 的"知道什么、记住什么、朝哪去"。

来源：https://github.com/DanieleSalatti/AgenticDesignPatterns
来源说明：各章配套 notebook 在该仓库 `notebooks/` 目录，按 `Chapter N_` 前缀分章；原书 PDF 在本目录 `Agentic_Design_Patterns.pdf`。

## Memory Management（第 8 章）

定义：区分短期与长期记忆的双组件系统。短期记忆保存最近交互数据（受上下文窗口约束），长期记忆用外部存储（通常是向量库）做语义检索，支持跨会话持久化。

适用条件：agent 需要多轮上下文、追踪多步任务进度、或按用户偏好个性化响应。Google ADK 的拆分可作参考：Session（会话线程）、State（临时数据）、MemoryService（长期知识检索）。

不适用条件（推断）：单轮无状态交互不需要；状态写入遵循框架约定（如 ADK 的 state_delta），直接改状态字典是常见错误。

## Learning and Adaptation（第 9 章）

定义：agent 根据新经验与新数据改变自己的思考、行动或知识，涵盖强化学习、监督/无监督学习、少样本适应、在线学习、基于记忆的学习等机制。

适用条件：环境动态变化、需要持续改进的场景，如个性化助手、交易机器人、欺诈检测。书中还收录 SICA 案例：agent 通过迭代修改自身源码并在基准上评估存档版本，实现自我改进。

不适用条件（推断）：该章大篇幅讲 PPO/DPO 等模型训练算法，属于训练侧而非 agent 设计侧；做应用层 agent 时只需吸收"基于经验反馈调整行为"的思想，不必引入训练管线。

## Goal Setting and Monitoring（第 11 章）

定义：为 agent 显式定义清晰可度量的目标，同时建立监控机制持续追踪进度与环境状态，形成"目标 → 监控 → 修正"的反馈闭环。

适用条件：agent 需自主执行多步任务、适应动态条件、且不依赖人类逐步把关时。目标建议用 SMART 原则定义；监控对象包括 agent 动作、环境状态与工具输出。

不适用条件（推断）：有人类逐步确认的流程不需要自主目标追踪；目标定义含糊时监控闭环没有意义，先写清指标再谈自主。

## Knowledge Retrieval / RAG（第 14 章）

定义：查询到达时先从知识库检索相关片段，把它们附加到提示词再交给 LLM，使其回答基于外部、可验证的数据，从"闭卷"变为"开卷"。

适用条件：回答需基于最新、私有或领域专属知识，如企业内部文档问答、客服机器人、需要引用来源的场景。进阶变体：GraphRAG 用知识图谱处理多源关系推理，Agentic RAG 加推理层主动验证与整合检索结果。

不适用条件（推断）：任务与私有知识无关时检索层只增加延迟；检索质量差会直接污染回答，需配 Evaluation 度量命中率。

## 组合要点

- 常见闭环：Goal 定义目标，Memory 提供历史，RAG 提供外部事实，监控反馈驱动 Learning 调整行为。
- 上下文工程（context engineering）贯穿四个模式：给模型的每一步都要精选、打包最相关的信息，防止注意力过载。
