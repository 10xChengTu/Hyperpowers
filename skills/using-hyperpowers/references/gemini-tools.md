# Gemini CLI Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use the
Gemini CLI equivalent:

| Skill references | Gemini CLI equivalent |
|-----------------|----------------------|
| `Read` | `read_file` |
| `Write` | `write_file` |
| `Edit` | `replace` |
| `Bash` | `run_shell_command` |
| `Grep` | `grep_search` |
| `Glob` | `glob` |
| `TodoWrite` | `write_todos` |
| `Skill` tool | `activate_skill` |
| `WebSearch` | `google_web_search` |
| `WebFetch` | `web_fetch` |
| `Task` tool | `@agent-name` |

## Subagent Support

Gemini CLI supports subagents via the `@` syntax. Use `@generalist` for
implementation and review tasks unless a more specific bundled agent is
available.

| Skill instruction | Gemini CLI equivalent |
|-------------------|----------------------|
| `Task tool (hyperpowers:implementer)` | `@generalist` with the filled `implementer-prompt.md` template |
| `Task tool (hyperpowers:spec-reviewer)` | `@generalist` with the filled `spec-reviewer-prompt.md` template |
| `Task tool (hyperpowers:code-quality-reviewer)` | `@generalist` with the filled `code-quality-reviewer-prompt.md` template |
| `Task tool (general-purpose)` | `@generalist` with the full prompt |

Fill every placeholder in the prompt template before dispatching the subagent.
