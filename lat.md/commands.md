# Commands

The four slash commands implement the [[workflow#Status Lifecycle]] state machine. Each command is defined in `.claude/commands/` as a markdown file Claude Code reads as instructions.

## spec-draft

Creates a new spec file from a one-line description. This is always the entry point — specs are never created by hand to ensure consistent formatting and slug derivation.

**Input**: `$ARGUMENTS` — one-line description of the feature to build. If empty, prompts the user.

**Output**: `specs/<slug>.md` with `Status: draft` and all four sections populated from the description.

**Slug derivation**: lowercase, hyphens for spaces, 4-5 most meaningful words. Appends `-2`, `-3` if the slug already exists.

**Quality bar**: The command fills in reasonable initial content, but the user is expected to read and refine the draft before running `/spec-review`. The draft is a starting point, not a finished spec.

## spec-review

Reviews a spec file against four quality criteria and either approves it (transitions to `reviewed`) or lists specific issues to fix.

**Input**: `$ARGUMENTS` — path to a spec file, e.g. `specs/add-word-count.md`.

**Criteria**: each of the four sections (Problem, Approach, Implementation, Verification) is evaluated independently. A spec must pass all four to be approved.

**Output**: a scored table showing Pass / Needs work per section, a list of specific issues (or "None"), and a APPROVED / NEEDS REVISION verdict.

**State transition**: only updates `Status: draft` → `Status: reviewed` on APPROVED. On NEEDS REVISION, status is unchanged and the user must edit the spec and re-run.

**Re-review**: if the spec is already `reviewed`, `implemented`, or `validated`, the command asks before proceeding.

## spec-implement

Implements the code described in a spec and commits the result.

**Input**: `$ARGUMENTS` — path to a reviewed spec file.

**Precondition**: warns if status is not `reviewed`. This is a soft gate — the user can override — to encourage proper sequencing without hard-blocking experimentation.

**Behavior**: reads the Implementation section, follows the numbered steps in order, works only within this repo, commits all changes, then updates `Status: reviewed` → `Status: implemented` in the spec file and commits that too.

**Deviations**: if any implementation step is ambiguous, the command makes the simplest reasonable choice and reports it in the completion summary. Deviations are never silent.

**Output**: a completion report listing files changed, any deviations from the spec, and one manually-confirmed verification item as a self-check.

## spec-validate

Runs every item in the spec's Verification section and reports pass/fail with evidence.

**Input**: `$ARGUMENTS` — path to an implemented spec file.

**Precondition**: blocks if status is not `implemented`. Unlike `/spec-implement`, this is a hard gate — partial validation of an un-implemented spec produces misleading results.

**Behavior**: for each Verification item, runs the command or checks the file/behavior and records the result with evidence (command run, output seen). Never marks an item pass without concrete evidence.

**State transition**: updates `Status: implemented` → `Status: validated` only if ALL items pass. Partial passes do not advance the status.

**Output**: a table of checks with results and evidence, an overall PASS / FAIL verdict, and next steps.
