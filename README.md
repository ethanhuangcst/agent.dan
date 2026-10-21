# agent.dan：个人智能体集群

本工程用来开发属于 Ethan 自己的智能体集群。每个智能体一个顶层目录，需求与规格放该智能体目录内；通用设计指南与跨智能体探索分开放。

## 目录

| 目录 | 作用 |
| --- | --- |
| `hcp-engagement-agent/` | 智能体：HCP 个性化互动助手（agent 文件与需求规格 `req-spec.md`） |
| `course-design-agent/` | 智能体/产品：SDD 与 agent 课程（`note.md`、`original.prompt.md`、已交付课程 `course-design-delivered/`） |
| `sdd.works.ethan/` | 智能体：ethan 教练（`ethan.md`） |
| `specs/agent-design-patterns/` | 设计智能体的通用指南（ROCKS 编写范式、Gulli 21 模式选型、主题笔记），入口 `00-index.md` |
| `spike/` | 跨智能体的探索笔记（如 QQ Doc、Google Drive CRUD 调研） |

## 约定

- 每个智能体一个顶层目录，智能体文件放 `<name>/<name>.md`，需求与规格放同一目录内，例如 `hcp-engagement-agent/req-spec.md`。
- 通用设计知识放 `specs/agent-design-patterns/`，不绑某个智能体；选型从 `specs/agent-design-patterns/00-index.md` 进。
- 跨智能体的探索笔记放 `spike/`。
- 新增智能体时：建顶层目录、更新本表。
