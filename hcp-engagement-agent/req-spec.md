# hcp-engagement-agent 需求规格（总结）

> as_of：2026-10-20
> 来源：`~/code/hcp-engagement/specs/`（`spec1.0/1.product-definition.md`、`spec1.0/3.architecture.md` §5.4、`agent/agent-function-spec.md`、`agent/agent-design.md`、`spec1.0/ToDo.md`）
> 修正（2026-10-20，用户提出）：部署形态（非 MCP；服务器内嵌与本地 agent 两种形态）、合规预审列入能力、对话能力界定。源规格未覆盖的修正项已在文中标注。
> 评估确认（2026-10-20）：MCP 暴露面收敛为数据面，决策面只走 Web App。见 §1 部署形态与 §4 MCP 暴露面。
> 评估确认二（2026-10-20）：`retrieve_academic` / `retrieve_compliance` 转为 `hcp-engagement-mcp` 对外工具（只读，`retrieve_compliance` 附租户与引用标注条件）；源设计中的决策面工具移除，采纳方案 A，源仓库另行修订。见 §4。
> 修正二（2026-10-20，用户提出）：本地形态不调服务器决策面；一人一策与合规预审由本地 agent 基于数据面工具（HCP 信息、洞察、学术、合规知识）本地生成与判定，方案含双引用并恒标 `mlr_status=draft_not_reviewed`。见 §2 本地形态边界。
> 修正三（2026-10-21，用户提出）：无服务器洞察时本地 agent 合成洞察草稿，恒标「本地合成草稿」、不写回、不冒充服务器 `doing_now`；§4 `get_hcp_insights` 行的理由措辞同步修订。见 §4。
> 本文件是上述文档的需求总结，供本仓库参考；权威功能表以 `agent/agent-function-spec.md`（F-AGT-001–030、I-AGT-001–010）为准。

## 1. 产品定位与部署形态

hcp-engagement-agent 是 HCP Personalized Engagement Assistant 的决策与对话层，包名 `@hca/hcp-engagement-agent`。上游是 `hcp-twin-mcp`（Twin/Insights 事实）与 `medical-kb`（学术与合规 RAG），下游是 Web（数字分身、HCP 洞察、一人一策、Agent Tab）。

分层位置：

```text
感知层 hcp-twin-mcp → Twin / Insights 事实
知识层 medical-kb → academic ‖ compliance
决策层 hcp-engagement-agent ← 本需求
表达层 Web 四个工作台
约束层 cn-hcp-compliance → Fail-fast
```

部署形态（修正）：

- hcp-engagement-agent 本身不是 MCP，也不实现 MCP server。产品内的 MCP 服务：`hcp-twin-mcp`（Twin/Insights 事实）、`hcp-engagement-mcp`（HCP 数据：`list_hcp` / `query_hcp` / `add_hcp`）。
- 服务器形态：作为内嵌 agent 部署在 hcp-engagement 服务器端，全部能力经 Web App（hcp.agent-mate.ai）供用户使用；MCP 入口只暴露数据面（§4）。
- 本地形态：用户可把它作为本地 agent 部署在自己的 agent 工具（例如 workbuddy），Capability 与 Knowledge 随 agent 文件进入宿主工具。本地能力范围：3 个数据面 tool + 只读 `get_hcp_insights` + 只读 `retrieve_academic` / `retrieve_compliance`（§4）；一人一策与合规预审由本地 agent 在本地生成与判定（修正二，见 §2）。
- 暴露面收敛原则：数据面（事实的读与受控写）可经 MCP 暴露；决策面（受监管内容生成、合规闸门）只走服务器内嵌 agent + Web App。两种部署形态由此不重合。基线判断是信任边界：本地 agent 的 prompt 与文件可被修改或注入，服务器无法验证 MCP 调用方的意图与上下文真实性。

## 2. 四大职责

| 职责 | 需求 |
| --- | --- |
| 一句话洞察 | `synthesizeDoingNow` 生成 `doing_now.summary`（1–3 句，可审计）+ 可选 `analysis`；数字分身列表/详情与 HCP 洞察页同源同字段；summary 空才写，`refresh=true` 强制覆盖；Agent 拥有展示权威（MCP Stage E 只写结构化草稿，不写最终 summary） |
| Engagement Options | `proposeOptions` 经薄 LangGraph 主路径生成 3–5 条方案；每条含动作/负责人/渠道/主题/成功信号/合规旁注/优先级 + `academic_refs[]` + `compliance_refs[]` + `mlr_status=draft_not_reviewed`；可经 `revise_engagement` 对话迭代 |
| 合规预审 | 方案生成后、正式 MLR 人工审核前做机器预审：通过 / 附条件 / 拒绝；结果写方案旁注，不替代正式 MLR。入口 `runComplianceGate`（修正后列为显式能力） |
| 对话 | `chat` 两种 mode：`open_chat` 通用开放对话（不绑分身、不强制 `hcpId`、不默认注入 Twin）；`revise_options` 一人一策页底短对话（必须绑 `hcpId` + `optionRunId`）。同一 `sessionId` 不混用 mode |

### 对话能力是什么

对话不是模型可调用的 tool，而是 agent 的交互形态（用户使用它的入口）：

- 服务器形态：对话由 `chat` API（I-AGT-003）承载，两种 mode（`open_chat` / `revise_options`）；对话循环由服务器代码提供，模型在循环内自主选 tool、生成回复。
- 本地形态（workbuddy 等）：对话循环由宿主 agent 工具提供；本 agent 以文件或 MCP 接入宿主，模型在宿主循环内自主选 tool。
- 界定结论：在 ROCKS 结构里，对话属于 Outcome 定义的交互类型（用户以什么方式用这个 agent、每类交互何时算完成）；Capability 层对应的是那组 tools 与包级 API。「会对话」来自宿主或服务器提供的 loop，不是 agent 自带的第 6 个 tool。
- 本地形态边界（修正二）：本地 agent 不调服务器决策面（MCP 无 `propose_engagement_options` / `precheck_compliance`）；一人一策与合规预审由本地 agent 基于数据面工具（`get_hcp_insights`、`retrieve_academic`、`retrieve_compliance`）本地生成与判定。本地方案含双引用并恒标 `mlr_status=draft_not_reviewed` 与「本地」标注；本地预审不替代服务器闸门与正式 MLR，下发必须经 Web App 服务器闸门与人审。

## 3. 提案主路径（硬约束流程）

```text
get_twin_insights → retrieve_academic ‖ retrieve_compliance
  → compliance_gate（无命中 → 降级或拒敏）
  → propose_engagement_options → cn-hcp-compliance Fail-fast
  → persist options（Postgres engagement_options）
```

- `retrieve_compliance` 在提案路径不可跳过；跳过即失败。
- 敏感互动无合规 chunk：拒答或标注「需人工合规确认」。
- 生成后 Fail-fast：通过 / 附条件 / 拒绝。
- 勿硬编码百步旅程；节点只保留强制合规与引用。

## 4. Capabilities（模型可用 Tools，固定 5 个）

| Tool | 作用 | 硬规则 |
| --- | --- | --- |
| `get_twin_insights` | 读 Postgres Insights（含 doing_now / opportunities / interest_directions / tags） | 不返回整包 twin.json |
| `retrieve_academic` | medical-kb 学术检索 | specialty/themes 过滤，结果带 citation |
| `retrieve_compliance` | medical-kb 合规检索 | 提案路径必调；tenant SOP 过滤 |
| `propose_engagement_options` | 生成 3–5 条 | 必须含双引用 |
| `revise_engagement` | 按反馈修订方案 | 写 session / options |

`synthesizeDoingNow` 是包级 API（I-AGT-001），不占工具名额；P0 不加第 6 个 Tool。

合规预审不是第 6 个 Tool：模型侧经 `retrieve_compliance` + 主路径闸门节点执行（§3），独立入口是包级 API `runComplianceGate`（I-AGT-004）。修正后把它列为显式能力（职责见 §2），能力载体不变。

### MCP 暴露面

名称映射（暴露名 ↔ 源规格名）：

| 暴露名 | 源规格名 |
| --- | --- |
| `get_hcp_insights` | `get_twin_insights`（读 Postgres Insights） |
| `retrieve_academic` | 同名（medical-kb API） |
| `retrieve_compliance` | 同名（medical-kb API） |
| `precheck_compliance` | `runComplianceGate`（I-AGT-004） |
| `propose_engagement_options` | 同名 |

暴露结论：

| 能力 | 暴露 | 理由 |
| --- | --- | --- |
| `list_hcp` / `query_hcp` | 保留 | 只读事实，无冲突 |
| `add_hcp` | 保留，属 act | 写 HCP 记录；本地 agent 调用前停下让人确认 |
| `get_hcp_insights` | 只读暴露（不带 `refresh` 写入） | 单一事实源：有服务器 `doing_now` 时本地原样引用，不自行改写；无服务器洞察时本地合成草稿，恒标本地草稿、不写回、不冒充服务器结果（修正三）；残留风险限于一致性，可用本地 agent 文件 Stop 块约束 |
| `retrieve_academic` | 只读暴露 | 无写副作用；出参带 citation；检索时不出站（dense 为进程内小模型，不调 LLM、不访问公开源） |
| `retrieve_compliance` | 只读暴露，附两个条件 | 条件一：`tenant_id` 只来自服务端鉴权上下文，禁止作工具入参；无租户绑定时只返回全局种子，租户 SOP 仅在有租户鉴权时可见。条件二：工具 description 写明返回是条款引用（`clause_id` / `authority` / `version`），不是预审判定，不替代 `precheck` 与正式 MLR |
| `propose_engagement_options` | 不暴露 | 服务器侧写入缺 session/tenant 溯源；`productContext` 可被本地伪造注入，生成可能超出说明书标签范围。一人一策由本地 agent 在本地生成（修正二），不经服务器写入 |
| `precheck_compliance` | 不暴露 | 闸门是主路径节点（`retrieve_compliance` 不可跳过）；独立暴露会让服务器闸门判定脱离主路径被反复触发。本地预审由本地 agent 基于条款引用自行判定（修正二），不取服务器闸门结果 |

检索工具事实（源 rag-design §10、§12）：`retrieve_*` 无网络端点，仅进程内函数；Qdrant 只绑 `127.0.0.1:6333`（绑定检查常开，health 报 `bind_safe`）；浏览器与本地 agent 均不直连。转为对外工具不改变这一点，MCP 服务进程是唯一网络边界。

裁决记录（采纳方案 A）：源设计 `specs/mcp2.0/mcp2-design.md` §3 原规划六工具含 `propose_engagement_options` 与 `precheck_compliance`，与本节结论冲突。裁决为移除这两个决策面工具，由 `retrieve_academic` / `retrieve_compliance` 替代，工具总数维持六个；`hcp-engagement` 仓库的 product-backlog、mcp2-design、mcp2-stories、mcp2-test、sprint-backlog、architecture 同步修订，修订在另一 workspace 执行。远程提案若立项，走独立 MCP（带鉴权与 tenant 作用域）或 Web App。

## 5. 对外 API（BFF 调用；浏览器只调 BFF）

| API | 输入 | 输出 | 副作用 |
| --- | --- | --- | --- |
| `synthesizeDoingNow` | `{ hcpId, refresh? }` | `doing_now`（summary / analysis? / evidence_refs? / as_of） | 写 insights.doing_now |
| `proposeOptions` | `{ hcpId, tenantId?, productContext? }` | `{ runId, options[3..5], gate_result? }` | 写 options，内部跑主路径 |
| `chat` | `{ mode, sessionId?, hcpId?, message, optionRunId?, attachments? }` | `{ sessionId, mode, messages[], options? }` | 写 session；revise 可改 options |
| `runComplianceGate` | `{ hcpId, optionRunId }` | `{ status: pass\|conditional\|reject, findings[] }` | 可回写 options 旁注 |
| `health` | `{}` | llm / database / medical_kb 状态 | 无 |

## 6. LLM 接入

- 所有生成路径只经 `LlmClient`（chat / chatStream / tool-calling）；默认 Provider 为 Qwen（DashScope OpenAI 兼容，`qwen-plus` 可配 `qwen-max`）；支持 `openai_compatible` 切换其它厂商；新增厂商只加 adapter，不改 Capabilities 与 BFF 路由。
- Key 仅服务端进程环境（`DASHSCOPE_API_KEY` / `LLM_API_KEY` 等）；浏览器禁止持有 Key；日志脱敏。
- Options / session / doing_now 元数据写 `llm: { provider, model }` 供审计。
- 缺 Key：启动失败或 `health=llm:down`；测试用 mock `LlmClient`，CI 不强制 live Key。

## 7. 合规与 Permissions 硬边界

- 不编造说明书外疗效。
- 院内活动旁注：机构同意 + 代表备案。
- 不用处方、统方、非公开 CRM。
- 输出不替代正式 MLR（仅闸门结果与旁注）。
- 产品/SOP 上下文可入检索，不写入 Twin。
- 标签消费：`profile.tags` T1 偏 MSL；行政/政策角色慎促销。

## 8. 持久化与上下文

- Postgres（香港，与 Twin 共用 `DATABASE_URL`）：`hcp_insights`（doing_now JSONB）、`engagement_options`（事务写）、`chat_sessions`（含 mode、messages；`open_chat` 落 `hcp_id=_agent_general`）。
- 浏览器 localStorage 另存会话索引与正文；附件仅进 chat/revise 请求上下文（摘要进 context，不默认可执行）。
- Context 保护：截断冗长 RAG；Insights 经 tool 注入；cn-hcp-pro 模板短载入，禁止全文塞 prompt。
- Zod 校验所有对外入参；结构化错误无堆栈（错误码表见功能规格 §2.5，如 `COMPLIANCE_REJECTED`、`LLM_TIMEOUT`、`CHAT_MODE_MISMATCH`）。

## 9. 非目标

- Twin 采集 / Playwright / 爬虫（属 hcp-twin-mcp）。
- Qdrant embedding 与 ingest 管线（属 medical-kb）。
- 正式 MLR 电子签、CRM 回写、自动办会、讲课费支付。
- 多 Agent 路由；为洞察与开放对话各建独立进程（同包分 mode）。
- 无证据向模型工具表扩到 6+ Tool（合规预审见 §4，不占工具名额）。
- 决策面能力（`propose_engagement_options`、`precheck_compliance`）经 `hcp-engagement-mcp` 暴露。远程提案若立项，走独立 MCP（带鉴权与 tenant 作用域）或 Web App。
- 浏览器直连 LLM。

## 10. 实现批次与验收

- 批次：MVP-2（LlmClient、synthesizeDoingNow、同源消费）；MVP-4（5 Tools、薄 LangGraph、Options、闸门、双 mode chat、附件）。当前状态均为近闭环（待正式 UAT）。
- LOW PRIORITY 后置（不挡验收）：流式输出（F-AGT-022）、租户产品上下文（F-AGT-024）。
- 验收（朱同玉案例，live provider）：同源 doing_now 可展示；Options 3–5 条 + 双引用 + 合规旁注；`open_chat` 通用问答；`revise_options` 可改当前方案；跳过 `retrieve_compliance` 必须失败。
