---
name: using-hyperpowers
description: Use when starting any conversation - establishes Hyperpowers' streamlined brainstorming to subagent implementation workflow
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute or review a specific task, skip
this skill.
</SUBAGENT-STOP>

# Using Hyperpowers

Hyperpowers is a two-phase Superpowers-style workflow:

1. `brainstorming` turns the idea into a clear design and spec.
2. `writing-plans` creates the plan and immediately hands it to
   `subagent-driven-development`.

The key behavioral difference from Superpowers is autonomy after design. Do not
insert extra human confirmation gates for spec review, plan review, worktree
choice, or execution mode unless the user explicitly asks for them.

## Instruction Priority

1. User's explicit instructions: highest priority.
2. Hyperpowers skills.
3. Default model behavior.

If the user asks for a worktree, direct edits on `master`, no commits, no tests,
or any other workflow constraint, follow that instruction.

## Skill Rules

Before implementation work, check whether a Hyperpowers skill applies:

- New feature, behavior change, app/site/tool build, or creative coding task:
  use `brainstorming` unless the conversation already contains an approved
  design or sufficiently precise requirements.
- Approved design or existing spec: use `writing-plans`.
- Existing implementation plan with independent tasks: use
  `subagent-driven-development`.

Only these workflow skills are active in Hyperpowers:

- `brainstorming`
- `writing-plans`
- `subagent-driven-development`

## Default Workspace Behavior

Explicit user instruction wins. Otherwise, before modifying project files,
create or reuse a dedicated branch.

Use direct `main`/`master` edits only when the user asks for them. Use a
worktree only when the user asks for one or project instructions require one.

## Platform Adaptation

Skills may mention Claude Code tool names. Non-Claude platforms should use the
tool mappings in:

- `references/codex-tools.md`
- `references/copilot-tools.md`
- `references/gemini-tools.md`

## Red Flags

These are Hyperpowers failures:

- writing code before the design is clear
- asking the user to review the written spec after design approval
- asking the user to choose between subagent and inline execution
- defaulting to a worktree when the user did not ask for one
- defaulting to `main` or `master` when the user did not ask for that
- reintroducing removed Superpowers workflows as active steps
