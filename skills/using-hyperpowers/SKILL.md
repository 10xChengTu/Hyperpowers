---
name: using-hyperpowers
description: Use when starting any conversation - establishes Hyperpowers' streamlined brainstorming to subagent implementation workflow and the supporting skill catalog
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute or review a specific task, skip
this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a Hyperpowers skill might apply to what
you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way
out of this.
</EXTREMELY-IMPORTANT>

# Using Hyperpowers

Hyperpowers is a Superpowers-style skill system with two parts:

1. A **simplified active workflow** (brainstorm → plan → subagent
   implementation, no extra human gates).
2. A **supporting skill catalog** ported from Superpowers (debugging, TDD,
   verification, code review, worktrees, parallel agents, writing skills,
   finishing a branch).

## Instruction Priority

1. **User's explicit instructions** (CLAUDE.md, GEMINI.md, AGENTS.md, direct
   requests) — highest priority.
2. **Hyperpowers skills** — override default model behavior where they conflict.
3. **Default model behavior** — lowest priority.

If the user asks for a worktree, direct edits on `main`, no commits, no tests,
or any other workflow constraint, follow that instruction.

## How to Access Skills

**In Claude Code:** use the `Skill` tool. When you invoke a skill, its content
is loaded — follow it directly. Never use `Read` on skill files.

**In OpenCode / Copilot CLI / Gemini CLI:** use the platform's native skill
loader (`skill`, `activate_skill`, etc.). See the references in this skill's
directory for tool-name mappings.

## Active Workflow Skills

Only these three skills define the visible Hyperpowers workflow:

- `hyperpowers:brainstorming` — turn an idea into a clear design and spec.
- `hyperpowers:writing-plans` — turn an approved design into an implementation
  plan and hand it off.
- `hyperpowers:subagent-driven-development` — execute the plan task-by-task
  through subagents, without intermediate human confirmation gates.

The key behavioral difference from Superpowers is autonomy after design. Do not
insert extra human confirmation gates for spec review, plan review, or
execution mode unless the user explicitly asks for them.

## Supporting Skills

Use these whenever the situation matches, regardless of workflow phase:

- `hyperpowers:systematic-debugging` — any bug, test failure, or unexpected
  behavior, before proposing fixes.
- `hyperpowers:test-driven-development` — when implementing any feature or
  bugfix, before writing implementation code.
- `hyperpowers:verification-before-completion` — before claiming work is done,
  fixed, or passing; before committing or opening a PR.
- `hyperpowers:requesting-code-review` — when finishing tasks, major features,
  or before merging, to verify work meets requirements.
- `hyperpowers:receiving-code-review` — when reading code review feedback,
  before acting on suggestions.
- `hyperpowers:using-git-worktrees` — when starting work that needs isolation
  from the current workspace.
- `hyperpowers:dispatching-parallel-agents` — when facing 2+ independent tasks
  with no shared state or sequential dependencies.
- `hyperpowers:finishing-a-development-branch` — when implementation is
  complete and you need to merge, open a PR, or clean up.
- `hyperpowers:writing-skills` — when creating, editing, or verifying skills.

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (`brainstorming`, `systematic-debugging`) — these
   determine HOW to approach the task.
2. **Discipline skills next** (`test-driven-development`,
   `verification-before-completion`) — these constrain HOW work is done.
3. **Implementation skills last** (`writing-plans`,
   `subagent-driven-development`, domain-specific skills) — these guide
   execution.

Examples:

- "Let's build X" → `brainstorming` first, then `writing-plans` →
  `subagent-driven-development`.
- "Fix this bug" → `systematic-debugging` first; if the fix is non-trivial,
  follow with `test-driven-development`.
- "Is this PR safe to merge?" → `requesting-code-review`.

## Skill Types

- **Rigid** (TDD, debugging, verification): follow exactly. Don't adapt away
  the discipline.
- **Flexible** (patterns, design): adapt principles to context.

The skill itself tells you which.

## Default Workspace Behavior

Explicit user instruction wins. Otherwise:

- After design approval in `brainstorming`, ask the user **exactly once**
  whether to use a git worktree or a dedicated branch in the current checkout.
  The default if the user does not choose is a dedicated branch.
- Record the chosen strategy in the spec so `writing-plans` and
  `subagent-driven-development` can reuse it without asking again.
- Use direct `main` / `master` edits only when the user explicitly asks for
  them.

## Platform Adaptation

Skills may mention Claude Code tool names. Non-Claude platforms should use the
tool mappings in:

- `references/codex-tools.md`
- `references/copilot-tools.md`
- `references/gemini-tools.md`

## Red Flags

These thoughts mean STOP — you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read the current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |

These are Hyperpowers workflow failures:

- writing code before the design is clear
- asking the user to review the written spec after design approval
- asking the user to choose between subagent and inline execution
- skipping the single worktree-vs-branch question after design approval
- asking the worktree question more than once (it belongs only in
  `brainstorming`)
- silently using a worktree when the user picked a branch (or the reverse)
- defaulting to `main` or `master` when the user did not ask for that
- reintroducing removed Superpowers workflows (`executing-plans`,
  `using-superpowers`) as active steps
