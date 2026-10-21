# agent.dan：个人智能体集群

本工程用来开发属于 Ethan 自己的智能体集群。每个智能体一个顶层目录；工程 specs 与设计指南分开放。

## 目录

| 目录 | 作用 |
| --- | --- |
| `hcp-engagement-agent/` | 智能体：HCP 个性化互动助手（决策与对话层） |
| `course-design/` | 智能体/产品：SDD 与 agent 课程（名字待定） |
| `specs/` | 工程 specs：需求、设计等，按智能体同名配对 |
| `agent-design-patterns/` | 设计智能体的通用指南（ROCKS 编写范式、Gulli 21 模式选型、主题笔记），入口 `index.md` |

## 约定

- 每个智能体一个顶层目录，智能体文件放 `<name>/<name>.md`，例如 `hcp-engagement-agent/hcp-engagement-agent.md`。
- 每个智能体的需求与设计放 `specs/<name>/`，与顶层目录同名配对，例如 `specs/hcp-engagement-agent/req-spec.md`。
- 通用设计知识放 `agent-design-patterns/`，不绑某个智能体；选型从 `agent-design-patterns/index.md` 进。
- 新增智能体时：建顶层目录、建 `specs/<name>/` 配对、更新本表。