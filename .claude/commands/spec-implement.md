---
description: Implement the code described in a spec file
argument-hint: specs/<file>.md
---

# Spec Implement

Implement the spec at `$ARGUMENTS`.

## Instructions

### Step 1: Read the spec

Read the full spec at `$ARGUMENTS` before writing any code.

### Step 2: Check the status

- `draft`: warn "This spec hasn't been reviewed. Run `/spec-review $ARGUMENTS` first. Proceed anyway? (y/n)"
- `implemented` or `validated`: warn "This spec is already implemented. Proceeding will overwrite existing work. Continue? (y/n)"
- `reviewed`: proceed normally.

### Step 3: Understand the full spec before coding

Read these sections carefully:
- **Implementation** — follow these steps in order
- **Files to Change** — modify only what's listed (plus unavoidable supporting files)
- **Verification** — keep these in mind as you implement; your code should satisfy each one

### Step 4: Implement

- Follow the numbered Implementation steps in order
- Work within this repo only
- If a step is ambiguous, make the simplest reasonable choice and note it in your report
- After implementing, do a quick self-check: does the code run without errors?

### Step 5: Commit

```bash
git add -A
git commit -m "feat: <spec title>

Implements spec: $ARGUMENTS

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
```

### Step 6: Update the spec status

Change `**Status**: reviewed` → `**Status**: implemented` in the spec file, then commit that change too:

```bash
git add $ARGUMENTS
git commit -m "chore: mark spec as implemented"
```

### Step 7: Report

```
## Implementation Complete: <title>

### Changes Made
<list each file changed with a one-line description>

### Deviations from Spec
<any steps done differently from the spec, and why — or "None">

### Self-Check
<one verification item you confirmed works manually>

Next step: `/spec-validate $ARGUMENTS`
```
