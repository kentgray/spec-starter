---
description: Validate the implementation against the spec's verification checklist
argument-hint: specs/<file>.md
---

# Spec Validate

Validate the implementation of the spec at `$ARGUMENTS`.

## Instructions

### Step 1: Read the spec

Read the full spec at `$ARGUMENTS`, focusing on the **Verification** section.

### Step 2: Check the status

- `draft` or `reviewed`: warn "This spec hasn't been implemented yet. Run `/spec-implement $ARGUMENTS` first."
- `validated`: say "This spec is already validated." Then show the current verification checklist as-is.
- `implemented`: proceed normally.

### Step 3: Run each verification check

Work through every item in the Verification section:

| Item type | How to check |
|-----------|-------------|
| Runnable command | Run it, capture output |
| File existence | Read or list the file |
| File content | Read the file and confirm the expected content |
| Observable behavior | Describe the concrete evidence you observed |

For each item, record:
- The check text from the spec
- ✅ PASS or ❌ FAIL
- The evidence: what command you ran, what output you got

### Step 4: Output the validation report

```
## Validation Report: <title>

| # | Check | Result | Evidence |
|---|-------|--------|----------|
| 1 | <verification item> | ✅ PASS / ❌ FAIL | <what you ran / what you saw> |
| 2 | <verification item> | ✅ PASS / ❌ FAIL | ... |

### Overall: PASS / FAIL
```

### Step 5: Act on the result

**If ALL checks pass:**
- Update `**Status**: implemented` → `**Status**: validated` in the spec file.
- Commit the update: `git add $ARGUMENTS && git commit -m "chore: mark spec as validated"`
- Tell the user:

```
Spec validated. Full cycle complete: draft → review → implement → validate ✅

You can now:
- Start a new feature with /spec-draft
- Look at specs/ to see your growing history of completed work
```

**If ANY check fails:**
- Do NOT update the status.
- List exactly what failed and why so the user knows what to fix.
- Tell the user: "Re-run `/spec-implement $ARGUMENTS` to address the failing checks, then re-validate."
