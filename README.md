# spec-starter

A minimal Claude Code project for learning spec-driven development.

## The idea

Before writing code, write a spec. A spec answers three questions:

1. **What problem am I solving?**
2. **How will I solve it?**
3. **How will I know it's done?**

Once the spec is reviewed and approved, you implement it. Then you validate it against the spec's own checklist. This forces you to think before you code and gives you a clear definition of "done."

## The 4 commands

| Command | What it does |
|---------|-------------|
| `/spec-draft "what to build"` | Writes a spec file from your description |
| `/spec-review specs/<file>.md` | Reviews the spec, approves or requests changes |
| `/spec-implement specs/<file>.md` | Implements the code described in the spec |
| `/spec-validate specs/<file>.md` | Checks the implementation against the spec's verification items |

## Try it

```bash
# 1. Open this repo in Claude Code
claude

# 2. Draft a spec
/spec-draft "add a function that reverses a string"

# 3. Review it
/spec-review specs/add-string-reversal.md

# 4. Implement it
/spec-implement specs/add-string-reversal.md

# 5. Validate it
/spec-validate specs/add-string-reversal.md
```

## Look at the example first

`specs/example-add-greeting.md` is a completed spec (status: `validated`) with its implementation in `hello.py`. Read it before writing your first spec to understand the format.

## What comes next

Once this loop feels natural, the same pattern scales:
- Specs can describe multi-file features, not just single scripts
- Review can involve human sign-off, not just Claude
- Validation can run automated test suites
- Multiple specs can be grouped into initiatives with dependency ordering

The [ADW pipeline](https://github.com/kentgray) is this same loop — automated end-to-end with a task queue, multi-agent orchestration, and overnight deployment pipelines.
