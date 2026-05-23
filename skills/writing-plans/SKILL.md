---
name: writing-plans
description: Use when you have a spec or clear requirements for a multi-step task, before implementation
---

# Writing Plans

Write a concrete implementation plan from the approved spec, then hand it
directly to subagent-driven implementation. This is the second Hyperpowers
phase.

**Announce at start:** "I'm using the writing-plans skill to create the
implementation plan and then execute it with subagents."

**Save plans to:** `docs/hyperpowers/plans/YYYY-MM-DD-<feature-name>.md`
Create the directory if needed.

User preferences for plan location override this default.

## No Intermediate Human Gates

After the plan is written and reviewed, do not ask:

- "Please review this plan before I continue."
- "Which execution mode do you want?"
- "Should I continue?"

Hyperpowers always continues into `subagent-driven-development` unless there is
a genuine blocker, unresolved ambiguity, or an explicit user instruction to
pause.

## Workspace Strategy

The workspace strategy is decided once in `brainstorming` and recorded in the
spec. **Do not ask the user again here.**

1. Read the spec's "Workspace Strategy" field.
2. Apply it before the first file write:
   - **Worktree:** create the worktree (sibling directory, branch named
     `hyperpowers/<topic-slug>`) and continue inside it.
   - **Dedicated branch:** `git switch -c hyperpowers/<topic-slug>` if not
     already on the right branch.
   - **`main` / `master` / current branch:** stay where you are.
3. If the spec is missing this field (older spec, or user skipped
   brainstorming), fall back to a dedicated branch and note the choice in the
   plan header.

Default branch flow when the spec says "dedicated branch":

```bash
git status --short
git branch --show-current
git switch -c hyperpowers/<topic-slug>
```

If already on a dedicated non-main branch for this work, continue there instead
of nesting branches. Do not reset, stash, or revert user changes as part of this
step. If branch/worktree setup cannot be done safely, stop and report the
blocker.

## Scope Check

If the spec covers multiple independent subsystems, split it into separate
plans. Each plan should produce working, testable software on its own.

## File Structure

Before defining tasks, map out which files will be created or modified and what
each file is responsible for.

- Follow established codebase patterns.
- Prefer focused files with clear interfaces.
- Keep changes tightly scoped to the requested behavior.
- Include targeted cleanup only when it directly supports the work.

## Task Granularity

Each task should be small enough for one subagent to complete and review
independently. A good task usually includes:

- exact files to create or modify
- failing test or verification step
- minimal implementation step
- command to verify behavior
- commit step, unless the user forbids commits

## Plan Document Header

Every plan MUST start with this header:

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use hyperpowers:subagent-driven-development to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about the approach]

**Workspace Strategy:** [branch/worktree/current-branch decision]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## No Placeholders

Never write:

- "TBD", "TODO", "implement later", or "fill in details"
- "Add appropriate error handling" without concrete behavior
- "Write tests for the above" without actual test content
- "Similar to Task N" instead of repeating needed details
- references to types, functions, or methods not defined by the plan

## Self-Review

After writing the plan:

1. **Spec coverage:** every requirement maps to at least one task.
2. **Placeholder scan:** remove vague or incomplete instructions.
3. **Type consistency:** names, signatures, and paths match across tasks.
4. **Buildability:** a fresh subagent can execute each task with only the task
   text and the context you provide.

If subagents are available, dispatch the plan document reviewer with
`plan-document-reviewer-prompt.md` and fix blocking issues. Do not ask the user
to review the plan unless they explicitly requested that gate.

## Execution Handoff

After saving and reviewing the plan:

- Invoke `hyperpowers:subagent-driven-development`.
- Dispatch implementation subagents task by task.
- Continue until all tasks are complete, blocked, or genuinely ambiguous.
