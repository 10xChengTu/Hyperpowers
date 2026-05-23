# Hyperpowers Contributor Notes

Hyperpowers is a local fork of Superpowers focused on a narrower autonomous
workflow:

1. Brainstorm the design.
2. Write the spec and plan, then implement through subagents without
   intermediate human confirmation gates.

When editing this repository:

- Keep the visible workflow limited to `brainstorming`, `writing-plans`, and
  `subagent-driven-development`.
- Keep `using-hyperpowers` as the bootstrap skill.
- Do not reintroduce the removed Superpowers workflows as active skills.
- Save generated design docs under `docs/hyperpowers/specs/`.
- Save generated implementation plans under `docs/hyperpowers/plans/`.
- Preserve the workspace strategy rule: explicit user instruction wins;
  otherwise use a dedicated branch before modifying project files.
- Keep manifests, hooks, and harness entrypoints using the `hyperpowers` name.

The upstream remote is named `upstream` and points to
`https://github.com/obra/superpowers`.
