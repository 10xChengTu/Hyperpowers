# Hyperpowers

Hyperpowers is a streamlined, more autonomous adaptation of
[Superpowers](https://github.com/obra/superpowers) for coding agents.

It keeps the Superpowers mechanism: skills bootstrap automatically, the agent
brainstorms before building, writes a concrete spec and implementation plan,
then uses subagents for implementation and staged review.

The difference is workflow pressure. Hyperpowers removes the intermediate human
confirmation gates after design approval. Once the user has accepted the design
or has already given sufficiently specific requirements, the agent writes the
spec, writes the plan, chooses the requested workspace strategy, and executes
through subagents without stopping for "review this spec", "review this plan",
or "which execution mode?" prompts.

## Workflow

1. **brainstorming** - Understand the current project, ask only necessary
   clarifying questions, explore alternatives, and get the design into a clear
   shape.

2. **automated implementation** - Write the spec, write the plan, resolve the
   workspace strategy, and run `subagent-driven-development` continuously until
   complete or genuinely blocked.

## Workspace Strategy

Hyperpowers follows explicit user instructions first:

- If the user asks for a worktree, create or use a worktree.
- If the user asks to edit directly on `main`, `master`, or the current branch,
  do that.
- If the user specifies another workflow, follow it.
- If the user gives no workflow instruction, create or reuse a dedicated branch
  for the work before modifying project files.

If branch or worktree setup is impossible because of repository state or tool
limits, the agent reports the blocker instead of silently changing strategy.

## Included Skills

- `using-hyperpowers`
- `brainstorming`
- `writing-plans`
- `subagent-driven-development`

The broader Superpowers library includes more workflows. Hyperpowers keeps only
the autonomous path described above.

## Source

This repository was initialized from `obra/superpowers` v5.1.0 and then adapted
into a local Hyperpowers fork.

## License

MIT License. See [LICENSE](LICENSE).
