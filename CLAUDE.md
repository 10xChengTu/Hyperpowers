# Hyperpowers Contributor Notes

Hyperpowers is a local fork of Superpowers focused on a narrower autonomous
workflow:

1. Brainstorm the design.
2. Write the spec and plan, then implement through subagents without
   intermediate human confirmation gates.

When editing this repository:

- Keep the **active workflow** limited to `brainstorming`, `writing-plans`, and
  `subagent-driven-development`. This trio replaces the Superpowers
  brainstorm→plan→executing-plans chain and must not gain extra human
  confirmation gates.
- Keep `using-hyperpowers` as the bootstrap skill (replaces upstream
  `using-superpowers`).
- The following Superpowers skills are **excluded** from this fork because they
  conflict with the simplified workflow: `executing-plans` (replaced by
  `subagent-driven-development`), `using-superpowers` (replaced by
  `using-hyperpowers`). Do not reintroduce them.
- All other Superpowers skills are **welcome as supporting skills**
  (`systematic-debugging`, `test-driven-development`,
  `verification-before-completion`, `requesting-code-review`,
  `receiving-code-review`, `using-git-worktrees`, `dispatching-parallel-agents`,
  `finishing-a-development-branch`, `writing-skills`). They may be added,
  updated, or rebased from upstream as long as they don't reintroduce the
  excluded workflow.
- When forking or updating a skill from upstream, rewrite `superpowers:`
  cross-references to `hyperpowers:` so the fork's branding stays consistent.
- Save generated design docs under `docs/hyperpowers/specs/`.
- Save generated implementation plans under `docs/hyperpowers/plans/`.
- Preserve the workspace strategy rule: explicit user instruction wins;
  otherwise use a dedicated branch before modifying project files.
- Keep manifests, hooks, and harness entrypoints using the `hyperpowers` name.

The upstream remote is named `upstream` and points to
`https://github.com/obra/superpowers`.
