# Spec Starter

This repo teaches spec-driven development through a 4-step loop. Every feature you build goes through the same sequence: **Draft → Review → Implement → Validate**.

## The 4 Commands (manual)

| Step | Command | What Happens |
|------|---------|--------------|
| 1 | `/spec-draft "what to build"` | Creates a spec file in `specs/` |
| 2 | `/spec-review specs/<file>.md` | Reviews the spec, approves or requests changes |
| 3 | `/spec-implement specs/<file>.md` | Implements the code from the spec |
| 4 | `/spec-validate specs/<file>.md` | Checks the implementation against the spec |

## The Orchestrator (after you've done the manual cycle)

```
/spec-build "what to build"
```

Runs all four steps in sequence. Two hard gates stop the pipeline if action is needed:
- **Review gate**: stops if the spec has issues — fix and re-run
- **Validate gate**: stops if any verification check fails — fix and re-run

One human checkpoint remains: after drafting, the pipeline shows you the spec and waits for your approval before implementing. Spec content is a design decision you should own.

## Why the Loop Matters

Writing the spec first forces you to answer three questions before touching code:
- **What problem am I solving?** (Problem section)
- **How will I solve it?** (Approach section)
- **How will I know it's done?** (Verification section)

The review step catches vague requirements before they become buggy implementations. Validation closes the loop — you only call something "done" when evidence says it works.

## Spec Format

Every spec lives in `specs/` and follows this shape:

```markdown
# Spec: <Title>
**Status**: draft | reviewed | implemented | validated

## Problem
What's missing or broken, and why does it matter?

## Approach
Strategy for solving it. Key design choices.

## Implementation
Numbered steps of the exact changes to make.

### Files to Change
| File | Change |
|------|--------|
| `path/to/file` | What changes and why |

## Verification
- [ ] Running `<command>` outputs `<expected result>`
- [ ] File `<path>` exists and contains `<expected content>`
```

## First-Time Walkthrough

```
/spec-draft "add a script that reverses a string"
/spec-review specs/add-string-reversal.md
/spec-implement specs/add-string-reversal.md
/spec-validate specs/add-string-reversal.md
```

Or study the completed example in `specs/example-add-greeting.md` alongside `hello.py`.

## Knowledge Graph

This project uses [lat.md](https://www.npmjs.com/package/lat.md) to document its design intent in `lat.md/`. Before starting work, search it:

```bash
lat search "what you're working on"   # semantic search
lat section "workflow#Spec Format"    # read a specific section
lat check                             # validate all links
```

The hook fires automatically on every prompt — you don't need to run `lat search` manually.

Key sections:
- `[[workflow]]` — the 4-step loop, spec format, status lifecycle, design decisions
- `[[commands]]` — inputs, outputs, state transitions, and edge cases for each command

After adding or changing commands or workflow behavior, update `lat.md/` and run `lat check`.
