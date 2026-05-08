---
description: Draft a new spec file from a one-line description
argument-hint: "what to build"
---

# Spec Draft

Create a new spec file for the feature described in `$ARGUMENTS`.

## Instructions

### Step 1: Get the description

If `$ARGUMENTS` is empty, ask: "What do you want to build? Describe it in one sentence."

### Step 2: Derive a file slug

Lowercase the description, replace spaces with hyphens, keep only the 4-5 most meaningful words.
Examples:
- "add a script that reverses a string" → `add-string-reversal`
- "build a word count command" → `word-count-command`

Check that `specs/<slug>.md` does not already exist. If it does, append `-2`, `-3`, etc.

### Step 3: Write the spec file

Create `specs/<slug>.md` using this exact template — fill in every section thoughtfully based on the description:

```markdown
# Spec: <Title>

**Status**: draft
**Created**: <today's date YYYY-MM-DD>

## Problem

<One paragraph: what's missing or broken, and why does it matter? Be specific. "It would be nice to have" is not a problem. What breaks or becomes harder without this?>

## Approach

<2-4 sentences: what strategy solves the problem? Mention any meaningful design choices. Do NOT describe implementation steps here — just the strategy.>

## Implementation

Numbered steps of the exact changes to make:

1. <Step one — name a specific file or function to create or change>
2. <Step two>
3. <Continue...>

### Files to Change

| File | Change |
|------|--------|
| `path/to/file.ext` | What will be added or modified |

## Verification

Each item must be a concrete, runnable check — not "it works":

- [ ] Running `<command>` outputs `<expected result>`
- [ ] File `<path>` exists and contains `<expected content>`
- [ ] <Other observable, checkable fact>
```

**Section guidance:**
- **Problem**: Why does this need to exist? What's the gap?
- **Approach**: What strategy solves it? Keep it short — no step-by-step yet.
- **Implementation**: Specific files and changes. A reader should be able to follow these steps without asking questions.
- **Verification**: Each item must be runnable or directly observable. At least 2 items.

### Step 4: Confirm

After writing the file, tell the user:
```
Created `specs/<slug>.md`.

Next step: `/spec-review specs/<slug>.md`
```
