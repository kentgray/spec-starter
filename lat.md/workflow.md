# Workflow

The spec-starter project teaches spec-driven development through a mandatory 4-step loop: every feature goes through Draft → Review → Implement → Validate before it can be called done.

## Why a Mandatory Loop

Each step is a prerequisite for the next, preventing the most common beginner failure modes.

Skipping spec-writing produces solutions to poorly-understood problems. Skipping review means vague requirements become bugs. Skipping validation means "done" is defined by feel, not evidence.

## Spec Format

A spec is a markdown file in `specs/` that answers three questions before any code is written.

Every spec has exactly four sections, in this order:

```
# Spec: <Title>
**Status**: draft | reviewed | implemented | validated

## Problem
## Approach
## Implementation
### Files to Change
## Verification
```

The `Status` field is the single source of truth for where a spec is in the lifecycle. Commands read and update it — never skip it.

### Problem Section

The Problem section answers: what is missing or broken, and why does it matter? A good problem statement is specific and explains the consequence of inaction. "It would be nice to have X" is not a problem. "Without X, Y breaks / Y becomes harder" is.

### Approach Section

The Approach section answers: what strategy solves the problem? It contains design decisions, not implementation steps. If there are no meaningful design choices, two sentences is enough.

### Implementation Section

The Implementation section answers: what exactly needs to change? Steps must be numbered and specific enough that a developer could follow them without asking questions. The Files to Change table makes the scope explicit before coding begins.

### Verification Section

The Verification section answers: how will we know it works? Every item must be a concrete, runnable command or directly observable fact. "It works" is not a verification item. At least two items are required.

## Status Lifecycle

```
draft → reviewed → implemented → validated
```

Each transition is controlled by a command:

| Transition | Command | Condition |
|------------|---------|-----------|
| `draft` → `reviewed` | `/spec-review` | All four sections pass quality checks |
| `reviewed` → `implemented` | `/spec-implement` | Code written and committed |
| `implemented` → `validated` | `/spec-validate` | All Verification items pass |

A status is never skipped. `/spec-implement` warns (but does not block) if status is not `reviewed`. `/spec-validate` blocks if status is not `implemented`.

## Design Decisions

Key choices in the workflow design and the reasoning behind them.

### Why review before implement?

Review catches two problems cheaply: vague problem statements and unrunnable verification items.

Both are far cheaper to fix before coding than after. A 5-minute review that blocks a bad spec saves hours of implementing the wrong thing.

### Why is Verification part of the spec?

Writing verification items before coding forces "done" to be defined upfront.

Without this, verification is retrofitted to match whatever was built. If you can't write a runnable verification item, the feature is underspecified.

### Why committed specs?

Specs are committed to git alongside code. This means every feature in the repo's history has a corresponding spec that explains why it was built. The spec is not documentation added after the fact — it is the prerequisite that gates implementation.

## Orchestrator

`/spec-build` is the graduation command — it runs all four steps in sequence for a zero-touch pipeline. It is designed to be used *after* the learner has done the manual cycle at least once.

The orchestrator has two hard gates where the pipeline stops and requires human action:

| Gate | Condition | Why it's a hard stop |
|------|-----------|----------------------|
| After Review | NEEDS REVISION | Implementing against a bad spec wastes all subsequent work |
| After Validate | Any check fails | Silent failures are worse than visible ones; do not push broken code |

One intentional human checkpoint remains even in zero-touch mode: after drafting, the orchestrator shows the spec and asks for confirmation before proceeding. Spec content represents design decisions that should not be automated away — the human needs to own what they're building.

### Connection to ADW

The `/spec-build` pipeline is a miniature version of the ADW (Agentic Developer Workflow) used in production:

| spec-build | ADW equivalent |
|------------|----------------|
| `/spec-draft` prompt | `/spec-create` + digest task registration |
| Review gate | Spec review before dispatch |
| `/spec-implement` | ADW agent runs `/implement` in target project |
| Validate gate | PR checks + architect verification |
| `git push` | PR auto-complete + merge |

The difference is scale: ADW operates across multiple projects with a task queue, overnight scheduling, and Azure DevOps pipelines. The loop is identical.
