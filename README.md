# agent.dan：个人智能体集群

本工程用来开发属于 Ethan 自己的智能体集群。每个智能体一个顶层目录；工程 specs 与设计指南分开放。

## 目录

| 目录 | 作用 |
| --- | --- |
| `hcp-engagement-agent/` | 智能体：HCP 个性化互动助手（agent 文件与需求规格 `req-spec.md`） |
| `course-design-agent/` | 智能体/产品：SDD 与 agent 课程（`note.md` 与已交付课程 `course-design-delivered/`） |
| `sdd.works.ethan/` | 智能体：ethan 教练（`ethan.rocks.md`） |
| `agent-design-patterns/` | 设计智能体的通用指南（ROCKS 编写范式、Gulli 21 模式选型、主题笔记），入口 `index.md` |
| `specs/` | 工程 specs：需求、设计等（`spike/` 探索、`course-design/` 原始需求） |

## 约定

- 每个智能体一个顶层目录，智能体文件放 `<name>/<name>.md`，例如 `hcp-engagement-agent/hcp-engagement-agent.md`。
- 智能体的需求与设计有两种放法：放自己的目录内（如 `hcp-engagement-agent/req-spec.md`），或放 `specs/<name>/` 与顶层目录同名配对（如 `specs/course-design/`）。
- 通用设计知识放 `agent-design-patterns/`，不绑某个智能体；选型从 `agent-design-patterns/index.md` 进。
- 新增智能体时：建顶层目录、更新本表。