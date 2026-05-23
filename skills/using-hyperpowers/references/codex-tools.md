# Codex Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use the
Codex equivalent:

| Skill references | Codex equivalent |
|-----------------|------------------|
| `Task` tool (dispatch subagent) | `spawn_agent` |
| Multiple `Task` calls | Multiple `spawn_agent` calls |
| Task returns result | `wait_agent` |
| Task completes automatically | `close_agent` to free slot |
| `TodoWrite` | `update_plan` |
| `Skill` tool | Skills load natively; follow the loaded instructions |
| `Read`, `Write`, `Edit` | Native file tools |
| `Bash` | Native shell tool |

## Subagent Dispatch

Hyperpowers requires subagent support for `subagent-driven-development`.

In Codex, use `spawn_agent`, `wait_agent`, and `close_agent`. Give each
implementation subagent the full task text from the plan; do not make the
subagent read the plan file and choose work itself.

## Workspace Detection

Before changing branches or deciding whether a worktree already exists, use
read-only git commands:

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
BRANCH=$(git branch --show-current)
```

- `GIT_DIR != GIT_COMMON` means you are already in a linked worktree.
- Empty `BRANCH` means detached HEAD.

Hyperpowers default: if the user gave no workflow instruction, create or reuse a
dedicated branch before modifying project files.
