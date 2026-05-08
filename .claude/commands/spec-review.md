---
description: Review a spec file and approve or request changes
argument-hint: specs/<file>.md
---

# Spec Review

Review the spec at `$ARGUMENTS` against four quality criteria. Approve it or list the issues to fix.

## Instructions

### Step 1: Read the spec

Read `$ARGUMENTS`. If the file doesn't exist, check `specs/` for a close name match.

### Step 2: Check the status

- If status is already `reviewed`, `implemented`, or `validated`: tell the user and ask if they want to re-review.
- If status is `draft`: proceed.

### Step 3: Evaluate each section

#### Problem
- [ ] Describes a real, specific problem — not vague ("improve the code", "it would be nice")
- [ ] Explains WHY it matters — what's broken or harder without this?
- [ ] Is concise (one paragraph or fewer)

#### Approach
- [ ] States a concrete strategy (not just "write some code")
- [ ] Mentions key design choices if any exist
- [ ] Does NOT contain implementation steps (those belong in Implementation)

#### Implementation
- [ ] Steps are numbered and specific — each names a file or function
- [ ] Files to Change table has real paths (not placeholders like `path/to/file`)
- [ ] A developer could follow these steps without asking clarifying questions

#### Verification
- [ ] Each item is a runnable command or directly observable fact
- [ ] No vague items like "it works" or "looks correct"
- [ ] Covers the happy path at minimum
- [ ] At least 2 items

### Step 4: Output the review

```
## Spec Review: <title>

### Section Scores
| Section | Result | Notes |
|---------|--------|-------|
| Problem | ✅ Pass / ⚠️ Needs work | <brief note> |
| Approach | ✅ Pass / ⚠️ Needs work | <brief note> |
| Implementation | ✅ Pass / ⚠️ Needs work | <brief note> |
| Verification | ✅ Pass / ⚠️ Needs work | <brief note> |

### Issues to Fix
<Numbered list of specific issues, or "None — spec is ready.">

### Decision
APPROVED  (or)  NEEDS REVISION
```

### Step 5: Act on the decision

**If APPROVED:**
- Update `**Status**: draft` → `**Status**: reviewed` in the spec file.
- Tell the user: "Spec approved. Next step: `/spec-implement $ARGUMENTS`"

**If NEEDS REVISION:**
- Do NOT change the status.
- Be specific about what to fix so the user knows exactly what to change before re-running this command.
