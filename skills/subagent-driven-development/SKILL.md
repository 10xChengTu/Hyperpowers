---
name: subagent-driven-development
description: Use when executing implementation plans with independent tasks in the current session
---

# Subagent-Driven Development

Execute the implementation plan by dispatching fresh subagents per task, with
staged review after each task.

Hyperpowers uses this continuously. Once a plan exists, do not pause for human
checkpoints between tasks. Stop only for a blocker, real ambiguity, a failed
workspace preflight, or completion.

## Preflight

Before dispatching implementation subagents:

1. Read the full plan once.
2. Extract every task with complete task text.
3. Confirm the workspace strategy in the plan has been applied.
4. If no explicit workspace strategy exists, apply the Hyperpowers default:
   create or reuse a dedicated branch unless the user requested worktree,
   main/master/current-branch edits, or another workflow.
5. Create a task checklist for the controller session.

Never start implementation on `main` or `master` unless the user explicitly
asked for it.

## Core Loop

For each task:

1. Record the current git SHA or working-tree baseline.
2. Dispatch an implementer subagent using `implementer-prompt.md`.
3. If the implementer asks for context, provide it and re-dispatch or continue.
4. If the implementer reports `BLOCKED`, resolve the blocker locally if
   possible. If it is a real ambiguity or architectural issue, stop and report.
5. Dispatch a spec compliance reviewer using `spec-reviewer-prompt.md`.
6. If spec review finds issues, send the issues back to the implementer and
   review again.
7. Dispatch a code quality reviewer using `code-quality-reviewer-prompt.md`.
8. If quality review finds blocking or important issues, send them back to the
   implementer and review again.
9. Mark the task complete only after both reviews pass.

## Continuous Execution

Do not ask "should I continue?" between tasks. The plan is the instruction to
continue.

Acceptable reasons to pause:

- a required user decision is missing and cannot be inferred
- branch/worktree setup is unsafe or impossible
- tests or verification reveal a blocker that changes the plan
- the implementation conflicts with explicit user instructions
- all tasks are complete

## Model Selection

Use the least powerful model that can handle the role:

- mechanical implementation in 1-2 files: fast model
- integration work across files: standard model
- architecture, difficult debugging, or review: strongest model available

## Handling Implementer Status

**DONE:** Proceed to spec compliance review.

**DONE_WITH_CONCERNS:** Read the concerns. Resolve correctness or scope doubts
before review; note non-blocking observations and continue.

**NEEDS_CONTEXT:** Provide the missing context and continue.

**BLOCKED:** Change something before retrying: add context, use a stronger
model, split the task, or stop if the plan is wrong.

Never ignore an escalation.

## Prompt Templates

- `implementer-prompt.md` - implementation subagent
- `spec-reviewer-prompt.md` - spec compliance reviewer
- `code-quality-reviewer-prompt.md` - code quality reviewer

## Finalization

After all tasks pass review:

1. Run the final verification commands from the plan.
2. Inspect `git status --short` and the final diff.
3. Dispatch a final code quality reviewer if the change is broad or risky.
4. Summarize what changed, what was tested, and any residual risk.

Do not invoke a separate finish-the-branch workflow; Hyperpowers intentionally
keeps the path to these two phases.

## Red Flags

Never:

- dispatch implementation before the plan exists
- let subagents choose their own task from the plan
- make subagents read the plan file instead of giving them the task text
- skip spec compliance review
- skip code quality review
- move to the next task with open review issues
- create parallel implementation subagents that might touch the same files
- start on `main` or `master` without explicit user instruction
