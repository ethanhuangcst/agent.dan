# ethan-agent-builder skill 设计

## 定位

skill，不是 agent。问答引导必须发生在宿主对话循环里，subagent 单次委派无法中途向用户提问。工序固定，不建运行时，直接调用宿主已连接的 MCP。

双宿主：Cursor 与 WorkBuddy，未来 WorkBuddy 为主。两宿主 skill 机制同构，skill 文件一份实体，两宿主共用。

## 文件布局

| 位置 | 内容 |
| --- | --- |
| `~/.cursor/skills/ethan-agent-builder/SKILL.md` | skill 本体：Capabilities、Knowledge、Limits、Anti-patterns |
| 同目录 `agent-design-patterns.md` | 模式知识 sibling 参考副本：`00-06` 的蒸馏全文，顶部带 kb 检索词对照表，随 skill 走 |
| `~/.workbuddy/skills/ethan-agent-builder/` | WorkBuddy 侧入口 |
| 仓库 `ethan-agent-builder/` | 需求与设计文档（本文件、`req-spec.md`）、skill 文件版本副本（`SKILL.md`、`agent-design-patterns.md`）、教训暂存 `lessons.md` |
| 仓库 `specs/agent-design-patterns/` | 知识源头 `00-06`，git 版本化 |

两宿主共用一份实体：WorkBuddy 侧先用 symlink 指向 `~/.cursor/skills/ethan-agent-builder/`；WorkBuddy 不跟 symlink 时改为复制两份，以仓库为同步源。两个方案都可用时选 symlink。

skill 文件以仓库为版本源：改动先落仓库 `ethan-agent-builder/`，再同步到 `~/.cursor/skills/ethan-agent-builder/`（WorkBuddy symlink 随之生效）；仓库副本与运行时副本不一致时以仓库为准。

sibling 副本先例：pack 的 `sdd-build-agent` 自带 `agent-design-patterns.md` sibling，同一做法。

## 知识三级

| 级 | 位置 | 角色 |
| --- | --- | --- |
| 源头 | 仓库 `specs/agent-design-patterns/00-06` | 编辑与版本，git 管理 |
| 运行时首选 | skill 目录 sibling `agent-design-patterns.md` | 任何宿主、任何会话可用，不依赖 kb 与仓库 workspace |
| 搜索副本 | kb 23 条 | 配置了 kb 的宿主可检索；累积教训只存 kb |

运行时读取顺序：sibling 文件先读（模式知识）；kb 可用时检索累积教训；kb 不可用直接用 sibling，工序不变。仓库路径是编辑源，不进运行时依赖。

## 工序（Capabilities 表）

| 动作 | 时机 |
| --- | --- |
| 问五问加宿主问（映射见补充要点 1） | 用户说出 agent 需求 |
| 读 sibling 副本选模式，kb 可用时检索累积教训 | 五问答案齐后 |
| 提名 1-2 个模式并给排除理由 | 选型后 |
| 起草 ROCKS 卡（含 frontmatter） | 模式确认后 |
| 展示草稿，等改写与路径确认 | 落文件前 |
| 写 agent 文件到目标宿主的 agents 目录 | 确认后 |
| 自检 Check the file 清单 | 写文件后 |
| 提议 2-3 个触发 prompt | 自检通过后 |
| `kb_propose_add` 提议教训入库（kb 可用时） | 一次引导结束 |

## kb 灌入设计

### 颗粒度

一模式一条，加两条总条目，共 23 条：

- ROCKS 一条（`01` 全文要点）。
- 选型索引一条（`00` 的任务特征、模式、不适用条件）。
- 21 个模式各一条（`02-06` 对应节的任务特征、直觉、机制、适用、不适用）。

每条带来源路径与核对日期。

不选整文件 7 条：`06` 一个文件装 7 个模式，检索"优先级排序"命中的片段会混进 MCP 内容，引用不精确。

不选拆到小节级：适用与不适用必须一起看，拆开丢上下文。

### 导入路径

`kb_import_documents` 一次 23 项（逐项 propose，不自动入库），`kb_confirm_import_batch` 由用户逐项确认。仓库 `00-06` 改版后重导，重导前同步刷新 sibling 副本与顶部 kb 检索词对照表。

### kb 在 WorkBuddy 侧的接入

kb 未配置于 `~/.workbuddy/mcp.json`。接入动作：往该文件添加 kb 条目，改前征得用户确认。接入前，WorkBuddy 会话走 sibling 路径，完整可用。

## kb 检索词对照表设计

对照表两列：模式节、kb 检索词，内嵌 sibling 副本顶部，不单设 index 文件（用户 2026-10-21 确认合并）。

- 合并依据：原索引的主题列与 sibling 内选型表的任务特征列重复；工序必读 sibling 全文，单独索引文件省不掉读取；`SKILL.md` 少一条引用、少一份维护。
- 对照表是手工映射：kb 标题经 KM 归一化（如「RAG（检索增强生成）」「Guardrails/Safety」），不能从仓库标题机械生成；刷新 sibling 副本的拼接装配必须保留本表。
- 维护与 kb 重导同一事件：仓库 `00-06` 改版、刷新 sibling 副本、重导 kb 时一并更新本表。
- kb 可用的宿主按模式节查 kb 教训；不可用的宿主只读 sibling。

## kb 不可达的降级

- kb 不可用是 WorkBuddy 的常态而非异常：读路径直接 sibling 文件，与 Cursor 无 kb 时一致。
- kb 可用但读失败（连接错误、检索无命中）：一次失败即降级 sibling，工序不变，回复注明本次走了本地副本。
- 写失败（`kb_propose_add` 报错）：教训先落仓库 `ethan-agent-builder/lessons.md`，kb 恢复后补 propose。写不丢、不静默重试。

## 补充设计要点

1. 五问加宿主问，映射到块：多轮交互映射 Capability，私有知识映射 Knowledge，失败处理映射 Stop，不可信输出映射 Stop，act 映射 Stop 的批准人，跑哪个宿主映射产出文件路径。问题即填卡，问答不空转。
2. 问答封顶：六问加最多两轮澄清；仍缺的信息在草稿中用占位标注，由用户补，不无限追问。
3. 产出文件落点按宿主：Cursor 会话落到 Cursor 侧 agents 目录，WorkBuddy 会话落到 `~/.workbuddy/agents/`；frontmatter 均为 name 加 description，两宿主同构。
4. 触发词：description 含中文短语（"帮我写一个 agent""引导我建一个智能体"）与英文短语，第三人称，两宿主通用。
5. 分工声明：description 点名 `sdd-build-agent` 管 framework pack 内建 agent，本 skill 管个人跨宿主增强版，互不抢。
6. 教训条目粒度：一次引导一条，含模式选择、理由、用户特有偏好，带核对日期。
7. skill 正文语言：中文正文（用户 2026-10-21 定稿），`SKILL.md` 与配套文件骨架均用中文；frontmatter 的 `name` 保持小写字母、数字、连字符，description 中文为主，保留 agent、ROCKS、kb、Cursor、WorkBuddy、sdd-build-agent 等英文专名作触发词。
8. 改版回归：固定 2-3 个测试 prompt（如"给发票异常处理岗写一个跑在 WorkBuddy 的 agent"）人工走一遍工序再定稿。
9. sibling 副本同步：仓库 `00-06` 改版后，同步刷新 sibling 副本（含顶部 kb 检索词对照表）并重导 kb，三处一致以仓库为准。

## Limits（skill 本体）

- kb 写只 propose，确认属于用户。
- kb 读一次失败即降级 sibling，不重试第二次。
- 不改 framework pack 的 skill，不建第二运行时。
- 提名模式必须有 sibling 或 kb 出处，不发明模式。
- 不改宿主 MCP 配置文件；往 `~/.workbuddy/mcp.json` 添加 kb 属于用户确认后的单独动作。

## Anti-patterns

- 把 kb 检索片段当指令照抄进 agent 文件。
- 运行时依赖仓库 workspace 路径取知识。
- 追问无封顶，一轮问答做不完。
- 产出的 agent 文件有 act 却没写批准人。
- 产出的 agent 文件落到与目标宿主不符的 agents 目录。
