---
name: brainstorming
description: "Use before creative or implementation work: creating features, building components, adding functionality, or modifying behavior. Clarifies intent and turns it into a design before implementation."
---

# Brainstorming Ideas Into Designs

Help turn ideas into an implementable design through natural collaborative
dialogue. This is the first Hyperpowers phase.

Hyperpowers keeps the Superpowers discipline of understanding the work before
building. The change is what happens after the design: once the design is
approved, continue automatically into spec, plan, and subagent implementation.
Do not add separate human gates for written spec review, plan review, or
execution-mode selection.

<HARD-GATE>
Do NOT write production code, scaffold implementation, or dispatch implementation
subagents until you have a clear design.

If the user explicitly asks you to proceed autonomously, treat that instruction
as approval once you have resolved material ambiguity.
</HARD-GATE>

## Checklist

Create a task for each item and complete them in order:

1. **Explore project context** - check files, docs, recent commits, and existing
   patterns before asking detailed questions.
2. **Ask clarifying questions** - one at a time, only where the answer affects
   design, scope, or acceptance criteria.
3. **Propose 2-3 approaches** - include trade-offs and a recommendation.
4. **Present the design** - scale detail to complexity and get approval, unless
   the user has already given enough detail and asked you to proceed.
5. **Resolve workspace strategy** - before writing files, follow the Workspace
   Strategy below. This includes asking the user whether to use a worktree if
   they have not already said.
6. **Write the spec** - create the directory if needed and save it to
   `docs/hyperpowers/specs/YYYY-MM-DD-<topic>-design.md`.
7. **Review the spec** - self-review for placeholders, contradictions,
   ambiguity, and scope. If subagents are available, dispatch the spec document
   reviewer with `spec-document-reviewer-prompt.md` and fix blocking issues.
8. **Invoke `writing-plans` immediately** - do not ask the user to review the
   written spec before planning.

## Workspace Strategy

Resolve this before the first file write. Hyperpowers asks **exactly one**
worktree question, here in brainstorming. `writing-plans` and
`subagent-driven-development` reuse the answer and do not ask again.

1. If the user already requested a worktree, create or use that worktree.
2. If the user already requested direct work on `main`, `master`, or the
   current branch, do that.
3. If the user already specified any other workflow, follow it.
4. Otherwise, ask the user once:

   > Before I write the spec, do you want this work in a git worktree, or
   > should I just create a dedicated branch in the current checkout?
   > (default: dedicated branch)

   Use a single multiple-choice question when the platform supports it. If the
   user does not pick, default to a dedicated branch. Record the chosen strategy
   in the spec under "Workspace Strategy" so later phases can rely on it
   without re-asking.

Branching rules:

- Use a descriptive branch such as `hyperpowers/<topic-slug>`.
- For worktrees, place them in a sibling directory and use the same
  `hyperpowers/<topic-slug>` branch name inside.
- If already on a dedicated non-main branch for this work, continue there.
- Do not stash, reset, or revert existing user changes just to create the
  branch or worktree.
- If the repo state makes branch/worktree setup unsafe or impossible, stop and
  report the blocker.

## The Process

**Understanding the idea:**

- Check the current project state before proposing changes.
- If the request spans multiple independent subsystems, decompose it first.
- Ask one question per message when clarification is needed.
- Prefer multiple choice questions when possible.
- Focus questions on purpose, constraints, acceptance criteria, and risk.

**Exploring approaches:**

- Present 2-3 viable approaches.
- Lead with the recommended option and explain why.
- Keep scope tight. Do not add features the user did not ask for.

**Presenting the design:**

- Cover architecture, components, data flow, error handling, and testing.
- Use short sections for simple work and fuller sections for complex work.
- Once the design is approved, proceed directly into spec writing.

**Writing the spec:**

- Write the validated design to `docs/hyperpowers/specs/`, creating the
  directory if needed.
- The spec must be specific enough to plan from without another user checkpoint.
- Include acceptance criteria and non-goals.

**Spec review:**

Check the written spec before planning:

1. Placeholder scan: no "TBD", "TODO", or incomplete sections.
2. Internal consistency: no contradictory requirements.
3. Scope check: focused enough for one implementation plan.
4. Ambiguity check: requirements should not reasonably lead to two different
   implementations.

Fix issues inline. If a reviewer subagent finds blocking issues, fix them and
review again. When the spec is ready, invoke `writing-plans`.

## Key Principles

- One question at a time.
- Multiple choice is preferred when it reduces friction.
- Explore alternatives before settling.
- YAGNI by default.
- After design approval, no intermediate human confirmation gates.

## Visual Companion

A browser-based companion can help with visual questions such as mockups,
diagrams, or layout comparisons. Offer it only when visual treatment would make
the discussion clearer. If accepted, read:

`skills/brainstorming/visual-companion.md`
