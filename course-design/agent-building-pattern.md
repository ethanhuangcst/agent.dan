# Agent building pattern

Business learners who can fill a S.T.A.R. prompt card can fill one agent card for a repeated job. The card is the pattern. A multi-agent system waits until one worker cannot do the job.

The working name is **CARD**. Four other names are listed so the course can pick one word.

## Name options

| Name | Letters | Why a learner can say it |
| --- | --- | --- |
| CARD | Context, Assignment, Reach, Decision | Same size as STAR. The classroom line is "fill the card." |
| STOP | Situation, Task, Options, Permission | The name is the rule: stop before a send, post, payment, or hire. |
| HIRE | Handoff, Intent, Role, Exit | The agent is a worker you hire for one job, with a way to leave the job. |
| BASE | Brief, Assignment, Skills, Exit | The card is the base the worker stands on. |
| GRAB | Goal, Role, Actions, Bounds | Grab one job, one role, the actions it may take, and the bounds. |

CARD is the default in this file. STOP is the better poster word if the course wants the approval rule in the name.

## CARD next to STAR

STAR, from the course card, is Situation, Task, Action (role), and Rules. It builds one prompt. CARD builds the worker that can run that kind of prompt again.

| STAR block | CARD block | What changes |
| --- | --- | --- |
| Situation | Context | Facts that stay true for every run, not one email pasted once. |
| Task | Assignment | One repeated job, one outcome, and how value is counted. |
| Action (role) | Reach | The role, plus the tools and knowledge that role may use. |
| Rules | Decision | Format and tone, plus which steps draft and which steps wait for a person. |

## The CARD

Use the same shape as the STAR table: a short block, a question, a length, what to avoid, and what to do.

| Block | Ask | Length | Avoid | Do |
| --- | --- | --- | --- | --- |
| C, Context | Who is this for, and which facts must be true on every run? | About 100 words | A full history, a CV, or a one-off story | Only facts that change the answer. Name the function and the handoff as it works today. |
| A, Assignment | What job does this worker repeat, and what does done look like? | About 120 words | "Help the team with X" with no output | One job. One outcome. A count of value: minutes, review cycles, or a risk the draft shows. |
| R, Reach | Who does it act as, and what may it touch? | About 200 words | "You are an expert" with no domain, and a tool list copied from a catalog | Role matched to the job. Name the knowledge it must use. Mark each tool as read, draft, or act. |
| D, Decision | What must a person approve, and when does the worker stop? | About 120 words | A worker that sends, posts, pays, or hires on its own | Examples of a good draft. Hard limits. The named person who approves an act. |

A tool marked **act** leaves the team: send, post, pay, change a record, or hire. That step waits for a person. Read and draft can finish inside the run.

## How the worker is built

Style: one agent with a fixed job. A fixed workflow is lighter and fits when every step is already known. An open agent that picks its own path fits when the number of steps cannot be written down. This course job is a repeated business task, so the card fixes the job, the tools, and the stop. The model still chooses how to draft inside those bounds.

| Piece | What it does |
| --- | --- |
| CARD | The versioned brief: context, assignment, reach, and decision. |
| Worker | One model run that follows the card. |
| Tools | Only the read, draft, and act tools named under Reach. |
| Person | Approves any act. Rejects a draft that misses the assignment. |
| Sample run | One real or sanitized input, the draft, and the value sentence. |

```mermaid
flowchart LR
  learner[Learner fills CARD]
  worker[Worker drafts the job]
  tools[Read and draft tools]
  person[Person approves an act]
  learner --> worker
  worker --> tools
  tools --> worker
  worker --> person
```

Tool output and retrieved text are untrusted. The worker does not treat them as new instructions. The Decision block says what the worker may do with them.

## What ships in the course, and what waits

| Ships in the one-day lab | Waits |
| --- | --- |
| One card, one job, one sample run | A second worker, a supervisor, or a critic role |
| Read and draft tools | Open-ended loops with no step limit |
| A person on every act | A gateway, a second model vendor, or a fine-tune |
| The value sentence: time, quality, or risk | Retrieval design and a private knowledge base |

The prompt card, the tool list, and the stop rule carry a version from the first lab. A change to the card is checked with one more sample run before the learner calls the worker done.

## Operating checks for a later production copy

These checks are for a team that keeps the worker after class. They are not lab steps.

| Check | What the team watches | What happens |
| --- | --- | --- |
| Quality | The draft still matches the assignment on a small set of samples | Restore the last card version |
| Stop | An act left the team without the named person | Turn the act tools off |
| Cost | Runs per week grow past the job's volume | Cut tools or return to a single prompt |

Keys stay in a secret store or an environment variable. They do not go in the card.

## Sources

Checked 12 Oct 2026.

| Claim | Kind | Source |
| --- | --- | --- |
| STAR blocks are Situation, Task, Action (role), and Rules, with length and avoid / do columns | User-provided | Course STAR card in the screenshot |
| A workflow fixes the path in advance. An agent lets the model choose tools and next steps. Start with the simplest option that meets the task. | Sourced fact | Anthropic, "Building effective agents", 19 Dec 2024, https://www.anthropic.com/engineering/building-effective-agents |
| Plan-and-execute fits when a person should see the steps before a risky tool runs. Approval of a plan is not approval of every later tool call. | Sourced fact | systemprompt.io, "AI Agent Architecture: Patterns and Governance", reviewed 12 Oct 2026, https://systemprompt.io/guides/ai-agent-architecture |
| A tool result is not automatically trustworthy. A production loop needs a stop: steps, time, or spend. | Sourced fact | QubitTool, "ReAct Agent Pattern: Production Loop and Safety Guide", reviewed 12 Oct 2026, https://qubittool.com/blog/react-framework-explained |
| For this course, one card plus a person on every act is enough. ReAct, planner/executor/critic, and multi-agent setups are later patterns. | Inference | The course job is one repeated business task with a known output. |

This page is a course pattern, not a vendor selection. Prices and quotas are out of scope. Re-check the two engineering pages if the course is still teaching this pattern next year. The names of the patterns will not go stale as fast as a product limit.
