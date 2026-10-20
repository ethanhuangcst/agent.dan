# Agent 设计模式总选型表

来源：《Agentic Design Patterns: A Hands-On Guide to Building Intelligent Systems》（Antonio Gulli），21 个模式章节。本表把 21 章压成一张选型表：先看任务特征，再选模式，最后用"不适用条件"排除。

来源：https://github.com/DanieleSalatti/AgenticDesignPatterns

## 选型表

| 任务特征 | 推荐模式 | 章号 | 不适用条件（推断） |
| --- | --- | --- | --- |
| 任务可拆成固定顺序的子步骤，前一步输出是后一步输入 | Prompt Chaining | 1 | 步骤间无依赖、或需要动态分支时，硬串行反而增加延迟 |
| 输入类型多样，需按意图分流到不同工具或子流程 | Routing | 2 | 只有一条执行路径、或分流规则少于两个时无收益 |
| 多个子任务互不依赖，且各自有 I/O 等待 | Parallelization | 3 | 任务强依赖彼此输出；或并发引入的调试与日志成本超过收益 |
| 需要访问模型训练数据之外的实时数据或执行动作 | Tool Use (Function Calling) | 5 | 纯文本生成任务，不需要外部数据或副作用 |
| 产出需要质量把关与迭代改进 | Reflection | 4 | 一次性低成本任务；或无明确评价标准，自我评估会空转 |
| 目标复杂，需要先分解为步骤序列再执行 | Planning | 6 | 单步可完成的简单任务，规划反而增加开销 |
| 单 agent 能力不足，任务可按专长分派给多个角色 | Multi-Agent Collaboration | 7 | 任务单角色可完成；多 agent 会放大协调成本与误差传播 |
| 需要跨会话记忆与上下文管理 | Memory Management | 8 | 单轮、无状态交互不需要 |
| 系统需根据历史经验持续改进行为 | Learning and Adaptation | 9 | 任务模式稳定，一次性调优已足够 |
| 需要以标准协议接入多种外部工具与数据源 | Model Context Protocol (MCP) | 10 | 工具少而固定时，直接 function calling 更简单 |
| agent 需自主执行多步任务并追踪目标达成 | Goal Setting and Monitoring | 11 | 有人类逐步把关的流程，不需要自主目标追踪 |
| 生产环境存在工具失败、网络错误等运行期故障 | Exception Handling and Recovery | 12 | 原型验证阶段，失败即可重跑的场景 |
| 决策后果涉及安全、伦理或大额资金，需人把关 | Human-in-the-Loop | 13 | 高吞吐低风险场景，人工介入会成为瓶颈 |
| 回答需基于私有或最新知识，且要求可溯源 | Knowledge Retrieval (RAG) | 14 | 任务与私有知识无关，或模型内知识已足够 |
| 多个异构框架的 agent 之间需要互操作与任务委派 | Inter-Agent Communication (A2A) | 15 | 全部 agent 在同一框架内，直接进程内调用即可 |
| 需在成本、延迟与质量之间动态权衡模型选择 | Resource-Aware Optimization | 16 | 流量小、预算宽松，简单单一模型足够 |
| 问题需要显式多步推理（数学、诊断、法律分析） | Reasoning Techniques (CoT/ReAct) | 17 | 模式化简单问答，显式推理只增加延迟 |
| agent 输出会影响用户、系统或声誉，需要防护 | Guardrails/Safety Patterns | 18 | 离线内部实验，输出无外溢风险 |
| 需要持续度量 agent 的效果、效率与合规 | Evaluation and Monitoring | 19 | 尚无生产部署，无运行指标可采集 |
| agent 需在冲突目标与有限资源下自主排序任务 | Prioritization | 20 | 同一时刻只有一个待办任务 |
| 开放式领域，需要发现"未知的未知" | Exploration and Discovery | 21 | 解空间已定义清楚的优化型任务 |

## 使用建议

- 先按"任务特征"列定位，再看"不适用条件"排除；排除依据多由各章失败模式反推，标注为推断，非原书原文。
- 模式可组合：生产系统常见组合是 Tool Use + Planning + Exception Handling + Evaluation 四件套。
- 按主题分组的详细笔记见同目录 01-05 五个文件。
