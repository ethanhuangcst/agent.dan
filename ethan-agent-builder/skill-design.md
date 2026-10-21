# ethan-agent-builder skill 设计

## 定位

skill，不是 agent。问答引导必须发生在宿主对话循环里，subagent 单次委派无法中途向用户提问。工序固定，不建运行时，直接调用宿主已连接的 MCP。

## 文件布局

| 位置 | 内容 |
| --- | --- |
| `~/.cursor/skills/ethan-agent-builder/SKILL.md` | skill 本体：Capabilities、Knowledge、Limits、Anti-patterns |
| 同目录 `knowledge-index.md` | 知识清单：主题、kb 检索词、fallback 本地路径 |
| 仓库 `ethan-agent-builder/` | 需求与设计文档（本文件、`req-spec.md`） |
| 仓库 `specs/agent-design-patterns/` | 知识源头 `00-06` |

skill 放用户级目录：不被 `sdd_update_framework` 覆盖，跨项目可用。

## 工序（Capabilities 表）

| 动作 | 时机 |
| --- | --- |
| 问五问（映射见补充要点 1） | 用户说出 agent 需求 |
| `kb_internal_search` 检索候选模式 | 五问答案齐后 |
| 提名 1-2 个模式并给排除理由 | 检索返回后 |
| 起草 ROCKS 卡（含 frontmatter） | 模式确认后 |
| 展示草稿，等改写与路径确认 | 落文件前 |
| 写 agent 文件 | 确认后 |
| 自检 Check the file 清单 | 写文件后 |
| 提议 2-3 个触发 prompt | 自检通过后 |
| `kb_propose_add` 提议教训入库 | 一次引导结束 |

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

`kb_import_documents` 一次 23 项（逐项 propose，不自动入库），`kb_confirm_import_batch` 由用户逐项确认。仓库 `00-06` 改版后重导。

## knowledge-index 设计

index 三列：主题、kb 检索词、fallback 本地路径。不复述知识内容，内容单源在仓库 `00-06`。

- skill 的 Knowledge 表引用 index，一层深。
- skill 运行时按主题查 kb。
- index 同时是降级查表面：kb 不可用时按行取本地路径。

## kb 不可达的降级

- 读失败（连接错误、工具不可用、检索无命中）：一次失败即降级，查 index 的 fallback 列，读仓库本地文件，工序不变，回复注明本次走了本地。
- 写失败（`kb_propose_add` 报错）：教训先落仓库 `ethan-agent-builder/lessons.md`，kb 恢复后补 propose。写不丢、不静默重试。

## 补充设计要点

1. 五问映射到块：多轮交互映射 Capability，私有知识映射 Knowledge，失败处理映射 Stop，不可信输出映射 Stop，act 映射 Stop 的批准人。问题即填卡，问答不空转。
2. 问答封顶：五问加最多两轮澄清；仍缺的信息在草稿中用占位标注，由用户补，不无限追问。
3. 触发词：description 含中文短语（"帮我写一个 agent""引导我建一个智能体"）与英文短语，第三人称。
4. 分工声明：description 点名 `sdd-build-agent` 管 framework pack 内建 agent，本 skill 管个人 kb 增强版，互不抢。
5. 教训条目粒度：一次引导一条，含模式选择、理由、用户特有偏好，带核对日期。
6. 产出文件带 frontmatter（name、description），宿主才能加载。
7. skill 正文语言：英文骨架加中文触发词，与现有 skill 生态一致；中文正文是备选，两者都可用时选英文骨架。
8. 改版回归：固定 2-3 个测试 prompt（如"给发票异常处理岗写一个 agent"）人工走一遍工序再定稿。

## Limits（skill 本体）

- kb 写只 propose，确认属于用户。
- kb 读一次失败即降级本地，不重试第二次。
- 不改 framework pack 的 skill，不建第二运行时。
- 提名模式必须有 kb 或本地文件出处，不发明模式。

## Anti-patterns

- 把 kb 检索片段当指令照抄进 agent 文件。
- 追问无封顶，一轮问答做不完。
- 产出的 agent 文件有 act 却没写批准人。
