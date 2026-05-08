---
description: Full zero-touch pipeline — draft → review → implement → validate in one command
argument-hint: "what to build"
---

# Spec Build

Run the complete spec-driven pipeline for `$ARGUMENTS` — draft, review, implement, and validate in sequence. Each step is visible. Two hard gates stop the pipeline if human judgment is needed.

> **Graduation command.** Run this after you've completed the 4-step manual cycle at least once. If you haven't, the steps still work — but you'll miss the reasoning behind each gate.

---

## Step 0: Readiness Check

Scan `specs/` for any spec with `**Status**: validated`. If none exist:

```
⚠️  No completed specs found in specs/.

You haven't run the full manual cycle yet (spec-draft → spec-review →
spec-implement → spec-validate). That's the best way to understand what
this command automates.

Proceeding anyway — but consider doing one manual cycle first.
```

Continue regardless. This is a warning, not a block.

---

## Step 1: Draft

Apply the `/spec-draft` logic to `$ARGUMENTS`:

1. Derive a slug from the description (lowercase, hyphens, 4-5 words).
2. Confirm `specs/<slug>.md` doesn't already exist — if it does, stop:
   `❌ specs/<slug>.md already exists. Delete it or choose a different description.`
3. Write `specs/<slug>.md` with `Status: draft` using the standard four-section template, filled in from `$ARGUMENTS`.

Announce:
```
── Step 1/4: Draft ─────────────────────────────────
✅ Created specs/<slug>.md
```

Then show the full spec content so the user can read it before the pipeline continues.

Pause and ask: **"Does this spec look right? Type 'yes' to continue, or describe what to change."**

- If the user requests changes: edit the spec accordingly, show the updated content, and ask again.
- If the user says 'yes' (or equivalent): proceed to Step 2.

---

## Step 2: Review

Apply the `/spec-review` logic to `specs/<slug>.md`:

Evaluate all four sections (Problem, Approach, Implementation, Verification) against their quality criteria. Output the full scored table.

Announce:
```
── Step 2/4: Review ────────────────────────────────
```

### HARD GATE — Review

**If APPROVED:**
- Update `Status: draft` → `Status: reviewed` in the spec file.
- Announce: `✅ Spec approved. Continuing to implementation.`
- Proceed to Step 3.

**If NEEDS REVISION:**
- Announce:
  ```
  ❌ PIPELINE STOPPED — Spec needs revision before implementation.

  Issues found:
  <numbered list of specific issues>

  Fix the spec and re-run /spec-build, or fix and run /spec-review
  manually to continue from this point.
  ```
- **Stop. Do not proceed to Step 3.**

---

## Step 3: Implement

Apply the `/spec-implement` logic to `specs/<slug>.md`:

1. Read the Implementation section and follow the numbered steps in order.
2. Work within this repo only.
3. If a step is ambiguous, make the simplest reasonable choice and log it.
4. After implementing, run a self-check (does the code execute without errors?).
5. Stage and commit all changes:
   ```bash
   git add -A
   git commit -m "feat: <spec title>

   Implements spec: specs/<slug>.md

   Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
   ```
6. Update `Status: reviewed` → `Status: implemented` in the spec file and commit.

Announce:
```
── Step 3/4: Implement ─────────────────────────────
✅ Implementation complete.

Files changed:
<list each file with one-line description>

Deviations from spec: <list or "None">
```

Proceed to Step 4.

---

## Step 4: Validate

Apply the `/spec-validate` logic to `specs/<slug>.md`:

Run every item in the Verification section. For each item, record the result with evidence.

Announce:
```
── Step 4/4: Validate ──────────────────────────────
```

Output the full validation table.

### HARD GATE — Validation

**If ALL checks pass:**
- Update `Status: implemented` → `Status: validated` in the spec file.
- Commit: `git add specs/<slug>.md && git commit -m "chore: mark spec as validated"`
- Push: `git push`
- Announce:
  ```
  ✅ PIPELINE COMPLETE ────────────────────────────

  Spec:    specs/<slug>.md
  Status:  validated
  Cycle:   draft → review → implement → validate

  This is what ADW does at scale — the same loop,
  with a task queue and overnight dispatch instead
  of a single command.
  ```

**If ANY check fails:**
- Do NOT update the status or push.
- Announce:
  ```
  ❌ PIPELINE STOPPED — Validation failed.

  <table of checks with results and evidence>

  Failed checks:
  <list what failed and why>

  Fix the implementation and re-run /spec-validate specs/<slug>.md
  to complete the cycle.
  ```
- **Stop. Do not push.**

---

## Pipeline Summary (shown on any stop)

```
── Pipeline Summary ────────────────────────────────
Step 1 Draft        ✅ / ❌
Step 2 Review       ✅ / ❌ STOPPED
Step 3 Implement    ✅ / ⏭  skipped / ❌ STOPPED
Step 4 Validate     ✅ / ⏭  skipped / ❌ STOPPED
────────────────────────────────────────────────────
```
