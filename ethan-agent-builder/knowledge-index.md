# Knowledge index

One row per topic. `kb 检索词` is the exact kb entry title in kb project `agent-design-patterns` (search with `kb_internal_search`). `本地参考` is the section in `agent-design-patterns.md` in this skill directory; the sibling file needs no kb and no repo workspace. This index repeats no pattern content; the content lives in the sibling file and the repo source `specs/agent-design-patterns/00-06` (editing source, agent.dan).

| 主题 | kb 检索词 | 本地参考 |
| --- | --- | --- |
| 为任务或系统选模式（选型入口） | Agent 设计模式选型索引（21 模式选型表） | 选型索引 → 选型表 |
| 编写单个 agent 定义文件（ROCKS 卡） | ROCKS（agent 编写范式） | ROCKS：agent 编写范式（全文） |
| 固定顺序流水线，前一步输出是后一步输入 | Prompt Chaining（提示词链，第 1 章） | 编排类 → Prompt Chaining（第 1 章） |
| 输入类型多样，按意图分流 | Routing（路由，第 2 章） | 编排类 → Routing（第 2 章） |
| 独立子任务并发执行 | Parallelization（并行化，第 3 章） | 编排类 → Parallelization（第 3 章） |
| 访问外部实时数据或执行动作 | Tool Use / Function Calling（工具使用，第 5 章） | 编排类 → Tool Use / Function Calling（第 5 章） |
| 产出需要质量把关与迭代改进 | Reflection（反思，第 4 章） | 自我改进类 → Reflection（第 4 章） |
| 目标复杂，先分解为步骤再执行 | Planning（规划，第 6 章） | 自我改进类 → Planning（第 6 章） |
| 按专长分派多个角色 | Multi-Agent Collaboration（多智能体协作，第 7 章） | 自我改进类 → Multi-Agent Collaboration（第 7 章） |
| 跨会话记忆与上下文管理 | Memory Management（记忆管理，第 8 章） | 记忆与上下文类 → Memory Management（第 8 章） |
| 根据历史经验持续改进行为 | Learning and Adaptation（学习与适应，第 9 章） | 记忆与上下文类 → Learning and Adaptation（第 9 章） |
| 自主执行多步任务并追踪目标达成 | Goal Setting and Monitoring（目标设定与监控，第 11 章） | 记忆与上下文类 → Goal Setting and Monitoring（第 11 章） |
| 回答基于私有或最新知识，要求可溯源 | RAG（检索增强生成） | 记忆与上下文类 → Knowledge Retrieval / RAG（第 14 章） |
| 工具失败、网络错误等运行期故障恢复 | Exception Handling and Recovery（异常处理与恢复，第 12 章） | 可靠性类 → Exception Handling and Recovery（第 12 章） |
| 高风险决策留给人把关 | Human-in-the-Loop / HITL（人在环路，第 13 章） | 可靠性类 → Human-in-the-Loop / HITL（第 13 章） |
| 输入输出多层安全防护 | Guardrails/Safety | 可靠性类 → Guardrails/Safety Patterns（第 18 章） |
| 以标准协议接入多种外部工具与数据源 | Model Context Protocol / MCP（模型上下文协议，第 10 章） | 运维与规模化类 → Model Context Protocol / MCP（第 10 章） |
| 异构框架 agent 之间互操作与任务委派 | Inter-Agent Communication / A2A（智能体间通信，第 15 章） | 运维与规模化类 → Inter-Agent Communication / A2A（第 15 章） |
| 成本、延迟与质量之间动态权衡模型选择 | Resource-Aware Optimization（资源感知优化，第 16 章） | 运维与规模化类 → Resource-Aware Optimization（第 16 章） |
| 显式多步推理（数学、诊断、法律分析） | Reasoning Techniques / CoT、ReAct（推理技术，第 17 章） | 运维与规模化类 → Reasoning Techniques（第 17 章） |
| 持续度量 agent 的效果、效率与合规 | Evaluation and Monitoring（评估与监控，第 19 章） | 运维与规模化类 → Evaluation and Monitoring（第 19 章） |
| 冲突目标与有限资源下自主排序任务 | Prioritization（优先级排序，第 20 章） | 运维与规模化类 → Prioritization（第 20 章） |
| 开放式领域，发现未知的未知 | Exploration and Discovery（探索与发现，第 21 章） | 运维与规模化类 → Exploration and Discovery（第 21 章） |
