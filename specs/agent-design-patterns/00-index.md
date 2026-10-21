# Agent 设计模式：选型索引

来源：《Agentic Design Patterns: A Hands-On Guide to Building Intelligent Systems》（Antonio Gulli），21 个模式章节。本索引把 21 章压成一张选型表：先看任务特征与示例，再选模式，最后用“不适用条件”排除。

- 原书全文：本目录 [Agentic_Design_Patterns.pdf](Agentic_Design_Patterns.pdf)
- 章号深读：社区中文翻译 [xindoo/agentic-design-patterns](https://github.com/xindoo/agentic-design-patterns) 的 `chapters/` 目录，下表“章号”链接直达该章中译；翻译与原书有出入时以 PDF 为准
- 配套代码：GitHub [DanieleSalatti/AgenticDesignPatterns](https://github.com/DanieleSalatti/AgenticDesignPatterns) 的 `notebooks/` 目录，按 `Chapter N_` 前缀分章；链接在 02-06 各主题笔记的模式节尾，标“配套代码”
- ROCKS（agent 编写范式）是本仓库自有内容，不是原书章节，见表首行与 [01-agent-writing-rocks.md](01-agent-writing-rocks.md)

## 选型表

| 任务特征 | 示例 | 推荐模式 | 章号 | 不适用条件（推断） |
| --- | --- | --- | --- | --- |
| 要定义或修订一个 agent（写 agent 文件） | 为发票异常处理岗写一个 agent 文件 | ROCKS（agent 编写范式） | [本仓库 01](01-agent-writing-rocks.md) | 运行期编排问题（串行、分流、并行）不归 ROCKS 管，按下面 21 个模式选 |
| 任务可拆成固定顺序的子步骤，前一步输出是后一步输入 | 市场分析流水线：摘要 → 提取趋势 → 撰写邮件 | Prompt Chaining | [第 1 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%201_%20Prompt%20Chaining.md) | 步骤间无依赖、或需要动态分支时，硬串行反而增加延迟 |
| 输入类型多样，需按意图分流到不同工具或子流程 | 客服机器人区分销售、技术支持、账号管理三类请求 | Routing | [第 2 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%202_%20Routing.md) | 只有一条执行路径、或分流规则少于两个时无收益 |
| 多个子任务互不依赖，且各自有 I/O 等待 | 同时调多个外部 API 取数，再汇总成一份报告 | Parallelization | [第 3 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%203_%20Parallelization.md) | 任务强依赖彼此输出；或并发引入的调试与日志成本超过收益 |
| 需要访问模型训练数据之外的实时数据或执行动作 | 查实时汇率、查数据库、发邮件、执行代码 | Tool Use (Function Calling) | [第 5 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%205_%20Tool%20Use.md) | 纯文本生成任务，不需要外部数据或副作用 |
| 产出需要质量把关与迭代改进 | 文案、代码、分析报告的“生成 → 评价 → 改进”循环 | Reflection | [第 4 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%204_%20Reflection.md) | 一次性低成本任务；或无明确评价标准，自我评估会空转 |
| 目标复杂，需要先分解为步骤序列再执行 | 自动生成研究报告、新员工入职流程、竞品分析 | Planning | [第 6 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%206_%20Planning.md) | 单步可完成的简单任务，规划反而增加开销 |
| 单 agent 能力不足，任务可按专长分派给多个角色 | 复杂研究分析、软件开发、创意内容生产 | Multi-Agent Collaboration | [第 7 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%207_%20Multi-Agent%20Collaboration.md) | 任务单角色可完成；多 agent 会放大协调成本与误差传播 |
| 需要跨会话记忆与上下文管理 | 跨会话记住用户偏好的个人助理；多步任务的进度追踪 | Memory Management | [第 8 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%208_%20Memory%20Management.md) | 单轮、无状态交互不需要 |
| 系统需根据历史经验持续改进行为 | 个性化助手、交易机器人、欺诈检测 | Learning and Adaptation | [第 9 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%209_%20Learning%20and%20Adaptation.md) | 任务模式稳定，一次性调优已足够 |
| 需要以标准协议接入多种外部工具与数据源 | 企业系统经 MCP 接入持续演进的外部工具与数据源 | Model Context Protocol (MCP) | [第 10 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2010_%20Model%20Context%20Protocol%20%28MCP%29.md) | 工具少而固定时，直接 function calling 更简单 |
| agent 需自主执行多步任务并追踪目标达成 | 自主执行多步任务，偏离目标时自我修正 | Goal Setting and Monitoring | [第 11 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2011_%20Goal%20Setting%20and%20Monitoring.md) | 有人类逐步把关的流程，不需要自主目标追踪 |
| 生产环境存在工具失败、网络错误等运行期故障 | 工具超时、API 报错时的重试、降级与回滚 | Exception Handling and Recovery | [第 12 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2012_%20Exception%20Handling%20and%20Recovery.md) | 原型验证阶段，失败即可重跑的场景 |
| 决策后果涉及安全、伦理或大额资金，需人把关 | 医疗、金融、自动驾驶的关键决策升级给人 | Human-in-the-Loop | [第 13 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2013_%20Human-in-the-Loop.md) | 高吞吐低风险场景，人工介入会成为瓶颈 |
| 回答需基于私有或最新知识，且要求可溯源 | 企业内部文档问答、带引用来源的客服机器人 | Knowledge Retrieval (RAG) | [第 14 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2014_%20Knowledge%20Retrieval%20%28RAG%29.md) | 任务与私有知识无关，或模型内知识已足够 |
| 多个异构框架的 agent 之间需要互操作与任务委派 | ADK、LangGraph、CrewAI 构建的 agent 互相委派任务 | Inter-Agent Communication (A2A) | [第 15 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2015_%20Inter-Agent%20Communication%20%28A2A%29.md) | 全部 agent 在同一框架内，直接进程内调用即可 |
| 需要在成本、延迟与质量之间动态权衡模型选择 | 简单查询给快而便宜的模型，复杂推理给强模型 | Resource-Aware Optimization | [第 16 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2016_%20Resource-Aware%20Optimization.md) | 流量小、预算宽松，简单单一模型足够 |
| 问题需要显式多步推理（数学、诊断、法律分析） | 多跳问答、代码调试、法律分析 | Reasoning Techniques (CoT/ReAct) | [第 17 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2017_%20Reasoning%20Techniques.md) | 模式化简单问答，显式推理只增加延迟 |
| agent 输出会影响用户、系统或声誉，需要防护 | 对外客服、内容生成平台、金融/医疗敏感系统 | Guardrails/Safety Patterns | [第 18 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2018_%20Guardrails_Safety%20Patterns.md) | 离线内部实验，输出无外溢风险 |
| 需要持续度量 agent 的效果、效率与合规 | 生产部署的准确率、延迟、解决率追踪 | Evaluation and Monitoring | [第 19 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2019_%20Evaluation%20and%20Monitoring.md) | 尚无生产部署，无运行指标可采集 |
| agent 需在冲突目标与有限资源下自主排序任务 | 多任务冲突时按紧迫性、重要性与成本动态排序 | Prioritization | [第 20 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2020_%20Prioritization.md) | 同一时刻只有一个待办任务 |
| 开放式领域，需要发现“未知的未知” | 科学研究假设生成、开放式市场机会发现 | Exploration and Discovery | [第 21 章](https://github.com/xindoo/agentic-design-patterns/blob/main/chapters/Chapter%2021_%20Exploration%20and%20Discovery.md) | 解空间已定义清楚的优化型任务 |

## 使用建议

- 先按“任务特征”列定位，再看“不适用条件”排除；排除依据多由各章失败模式反推，标注为推断，非原书原文。
- 模式可组合：生产系统常见组合是 Tool Use + Planning + Exception Handling + Evaluation 四件套。
- 按主题分组的详细笔记见同目录 02-06 五个文件；ROCKS 编写范式见 [01-agent-writing-rocks.md](01-agent-writing-rocks.md)。
