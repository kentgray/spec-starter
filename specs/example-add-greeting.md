# Spec: Add Greeting Script

**Status**: validated
**Created**: 2026-05-08

## Problem

This repo has no runnable code. A beginner needs one concrete, minimal example to follow through the entire spec → implement → validate cycle before writing their own specs. Without a reference, the format feels abstract.

## Approach

Create a small Python script (`hello.py`) that takes a name as a command-line argument and prints a greeting. Intentionally trivial — the goal is practicing the workflow, not building something complex.

## Implementation

1. Create `hello.py` in the repo root with a `greet(name: str) -> str` function that returns `"Hello, {name}!"`.
2. Add a `__main__` block that reads `sys.argv[1]` as the name and prints the result.
3. Handle the missing-argument case: print `"Usage: python hello.py <name>"` and exit with code 1.

### Files to Change

| File | Change |
|------|--------|
| `hello.py` | Create new file with `greet()` function and CLI entry point |

## Verification

- [ ] Running `python hello.py Alice` outputs `Hello, Alice!`
- [ ] Running `python hello.py` (no args) outputs `Usage: python hello.py <name>` and exits with code 1
- [ ] File `hello.py` exists and contains a function named `greet`
