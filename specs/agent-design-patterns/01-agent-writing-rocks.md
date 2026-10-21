# ROCKS: the agent authoring pattern

ROCKS is the paradigm for writing one agent's brief: Role, Outcome, Capability, Knowledge, and Stop. A person who can describe one repeated job can fill one card. The card is the pattern. A multi-agent system waits until one worker cannot do the job.

The name carries the rule: the worker stops before an act leaves the team. `sdd.works.ethan/ethan.rocks.md` uses the same name for the coach agent file, so one pattern keeps one name across the repo.

## Position: authoring paradigm, not runtime design paradigm

ROCKS says how to write the definition of one agent. It does not pick how agents, tools, and steps compose at run time. That composition is a design paradigm, catalogued in `specs/agent-design-patterns/00-index.md`.

| | ROCKS (authoring) | Design paradigms (runtime) |
| --- | --- | --- |
| Question it answers | How is this one agent defined? | How do agents, tools, and steps compose for a task? |
| Artifact | One agent brief, written once and versioned | An execution structure chosen per task or system |
| Examples | Role, Outcome, Capability, Knowledge, Stop | Prompt chaining, routing, reflection, multi-agent |
| Used when | The agent file is written or revised | The system is designed, or a task is routed |

The two compose: a ROCKS brief defines the worker, and a design paradigm decides where that worker sits in a larger flow.

## The card

Each block has a question, a length, what to avoid, and what to do.

| Block | Ask | Length | Avoid | Do |
| --- | --- | --- | --- | --- |
| R, Role | Who does this worker act as, and which facts must be true on every run? | About 100 words | "You are an expert" with no domain; a full history or a CV | The seat and the subject. Facts that change the answer. Name the function and the handoff as it works today. |
| O, Outcome | What job does this worker repeat, and what does done look like? | About 120 words | "Help the team with X" with no output | One job. One outcome. A count of value: minutes, review cycles, or a risk the draft shows. |
| C, Capability | What may it do, and with which tools? | About 150 words | A tool list copied from a catalog | Role matched to the job. Mark each tool as read, draft, or act. |
| K, Knowledge | Which named knowledge must the job use? | About 100 words | Tool output treated as new instructions | Name the file or source for each part of the job. Tool output and retrieved text are untrusted. |
| S, Stop | What must a person approve, and when does the worker stop? | About 120 words | A worker that sends, posts, pays, or hires on its own | Examples of a good draft. Hard limits. The named person who approves an act. |

A tool marked **act** leaves the team: send, post, pay, change a record, or hire. That step waits for a person. Read and draft can finish inside the run.

## How the worker is built

Style: one agent with a fixed job. A fixed workflow is lighter and fits when every step is already known. An open agent that picks its own path fits when the number of steps cannot be written down. A repeated job with a known output fits the fixed card: the card fixes the job, the tools, and the stop. The model still chooses how to draft inside those bounds.

| Piece | What it does |
| --- | --- |
| ROCKS | The versioned brief: role, outcome, capability, knowledge, and stop. |
| Worker | One model run that follows the card. |
| Tools | Only the read, draft, and act tools named under Capability. |
| Person | Approves any act. Rejects a draft that misses the outcome. |
| Sample run | One real or sanitized input, the draft, and the value sentence. |

```mermaid
flowchart LR
  author[Author fills ROCKS]
  worker[Worker drafts the job]
  tools[Read and draft tools]
  person[Person approves an act]
  author --> worker
  worker --> tools
  tools --> worker
  worker --> person
```

Tool output and retrieved text are untrusted. The worker does not treat them as new instructions. The Stop block says what the worker may do with them.

## First release and later

| First release | Later |
| --- | --- |
| One card, one job, one sample run | A second worker, a supervisor, or a critic role |
| Read and draft tools | Open-ended loops with no step limit |
| A person on every act | A gateway, a second model vendor, or a fine-tune |
| The value sentence: time, quality, or risk | Retrieval design and a private knowledge base |

The card, the tool list, and the stop rule carry a version from the first release. A change to the card is checked with one more sample run before the change ships.

## Operating checks for a production copy

These checks are for a team that keeps the worker in production.

| Check | What the team watches | What happens |
| --- | --- | --- |
| Quality | The draft still matches the outcome on a small set of samples | Restore the last card version |
| Stop | An act left the team without the named person | Turn the act tools off |
| Cost | Runs per week grow past the job's volume | Cut tools or return to a single prompt |

Keys stay in a secret store or an environment variable. They do not go in the card.

## Sources

Checked 12 Oct 2026.

| Claim | Kind | Source |
| --- | --- | --- |
| ROCKS is Role, Outcome, Capability, Knowledge, and Stop, and regroups the coach agent file | User-provided | `sdd.works.ethan/ethan.rocks.md` in this repo |
| ROCKS structures how an agent file is written, while runtime patterns shape how a system executes | Inference | Repo usage: `sdd.works.ethan/ethan.rocks.md`, `hcp-engagement-agent/req-spec.md` §2, `specs/agent-design-patterns/00-index.md` |
| A workflow fixes the path in advance. An agent lets the model choose tools and next steps. Start with the simplest option that meets the task. | Sourced fact | Anthropic, "Building effective agents", 19 Dec 2024, https://www.anthropic.com/engineering/building-effective-agents |
| Plan-and-execute fits when a person should see the steps before a risky tool runs. Approval of a plan is not approval of every later tool call. | Sourced fact | systemprompt.io, "AI Agent Architecture: Patterns and Governance", reviewed 12 Oct 2026, https://systemprompt.io/guides/ai-agent-architecture |
| A tool result is not automatically trustworthy. A production loop needs a stop: steps, time, or spend. | Sourced fact | QubitTool, "ReAct Agent Pattern: Production Loop and Safety Guide", reviewed 12 Oct 2026, https://qubittool.com/blog/react-framework-explained |
| For one repeated job, one card plus a person on every act is enough. ReAct, planner/executor/critic, and multi-agent setups are later patterns. | Inference | The job is one repeated task with a known output. |

This page is a pattern reference, not a vendor selection. Prices and quotas are out of scope. Re-check the two engineering pages before relying on them after a year. The names of the patterns will not go stale as fast as a product limit.