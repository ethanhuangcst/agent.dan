# Note: three course designs

This note records the design request from 12 Oct 2026 and the choices written into the three course files.

## Files

- `course-design/01-one-day-agent-workshop.md`: one-day agent workshop for business roles
- `course-design/02-one-day-sdd-workshop.md`: one-day Spec-Driven Development (SDD) workshop for product and change roles
- `course-design/03-two-day-sdd-workshop.md`: two-day SDD workshop for engineers, fixed at nine modules

## Request

Ethan is designing three courses.

### One-day agent workshop, light SDD

Audience: not technical. Business roles such as finance, sales, marketing, supply chain, manufacturing, HR, and leadership.

After the day, learners will be able to:

- Use Markdown to communicate with a large language model (LLM)
- Use an S.T.A.R. prompt for a complex task
- Build an AI agent that creates business value
- Optional: practice SDD to build a simple app

### One-day SDD workshop

Audience: not hands-on technical. People between business and a software team, such as Product Owners, business analysts, and digital transformation teams.

After the day, learners will be able to:

- Define SDD
- Explain how SDD, agentic programming, and harness engineering relate
- List the four cores: skills, rules, knowledge, and workflow
- Define agent, MCP (Model Context Protocol), RAG (retrieval-augmented generation), and application
- Design a workspace that holds specs and the product-building bundle
- Use skills to build a simple application from end to end: ATDD (acceptance-test-driven development), UI design and mock-up, technical design, testing, and implementation
- Use skills and rules to manage project knowledge
- Examine how to create a workflow and recommend a pattern

### Two-day SDD workshop

Audience: technical. Software engineers moving from classic coding or vibe coding to SDD and harness engineering.

After the two days, learners will be able to:

- Define SDD, agentic programming, and harness engineering
- List the four cores
- Define agent, MCP, RAG, and application
- Use skills to build a simple application from end to end
- Build a simple RAG solution as an MCP service
- Build a local custom agent
- Design a Scrum-in-SDD framework that joins SDD and Scrum under harness principles, using skills, rules, and knowledge
- Practice that framework by building a simple RAG MCP service

The last two outcomes are different. One is the framework design. The other is using that framework on the class build. Building the RAG service is a third outcome: the running service.

Rough journey Ethan described:

- Part 1: learning outcomes 1 to 4, with framework.sdd.works Lite skills and rules, by building a simple web app
- Part 2: design a Scrum-in-SDD framework for the learner's own team
- Part 3: class project for the remaining outcomes, a RAG MCP service
- Part 4: mini-workshop on an org-level framework

Framework constraints Ethan set for Part 2:

- Incremental delivery
- Backlog refinement: feature breakdown, sizing, and feature detail with ATDD
- Product backlog and sprint backlog
- Frequent progress inspection
- Scrum events: retrospective is required, sprint planning is optional, and the room examines whether a daily scrum is still needed
- An org-level knowledge base for all teams

What learners do in Part 2: break-out work on separate Miro boards, and list the skills, rules, and services in the design.

Open point Ethan left: whether learners author one or two skills, or receive the full framework.sdd.works pack.

Part 3 artifacts:

- Process files: `product-backlog.md`, `sprint-backlog.md`, `status.md`
- Engineering files: `architecture.md`, `.secrets`, `test-strategy.md`, `release.md`
- Harness skills: `sdd-update-project`, `sdd-update-specs`, `sdd-review-status`, `sdd-plan-sprint`, `sdd-refine-backlog`, `sdd-spec-to-build`, `sdd-retrospective`

Hard limit: the two-day journey is nine modules, with weight that fits two days.

Each course file also holds five candidate names, an objective paragraph, refined learning objectives, and a module design.

## Choices in the drafts

- S.T.A.R. means Situation, Task, Action, Result. Confirm if you use a different expansion.
- Bloom's taxonomy (a scale from recall to creation) is named on each objective. Verbs match the evidence a learner can show in the room.
- The two-day file has exactly nine modules. Day 1 is modules 1 to 4. Day 2 is modules 5 to 9. Contact time is about six hours each day, excluding lunch.
- Learning outcome 5 and learning outcome 8 share one class project and use different evidence. Outcome 5 is a running RAG MCP. Outcome 8 is the backlog, status, and retrospective produced while that slice is built.
- Part 4 is learning outcome 9 in the two-day file, so the ninth module has a checkable result.
- Recommendation on the open point: give the full framework.sdd.works pack for the class build. In the Miro break-out, teams list skills, rules, and services. They do not author a new skill inside the nine modules. A short look at one skill file shows the shape. Authoring a skill would replace time on the RAG slice.

## Names, revised 12 Oct 2026

The first name lists described the syllabus. Ethan asked for names that are attractive and that state the value from the learner's side.

His drafts:

- AI for value: move from claw to agents
- From Idea to launch: A Product Owner needs to know in the AI era
- Agentic Programming with SDD: under Harness principles

"Claw" is read here as the chat assistant people already use. The value is the move from that chat to an agent that returns work. If claw is a product name that must stay on the poster, the business title changes back to include it.

Working titles now:

- Business: AI for Value: from chatbot to a worker
- Product and change: From Idea to Launch: leave with a slice you can show
- Engineers: Under a Harness: agentic work your sprint can trust

## Open points

- Pick one name from each list of five, or send a replacement.
- Confirm the S.T.A.R. expansion.
- Confirm the full-pack recommendation, or switch the two-day lab to authoring one skill and name which module loses time.
- Daily scrum stays an examination in module 6. The draft does not remove the event.
