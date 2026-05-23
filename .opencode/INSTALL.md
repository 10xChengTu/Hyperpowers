# Installing Hyperpowers for OpenCode

## Prerequisites

- [OpenCode.ai](https://opencode.ai) installed

## Local Installation

Add Hyperpowers to the `plugin` array in your `opencode.json`:

```json
{
  "plugin": ["/Users/zhenghui/Documents/repos/Hyperpowers"]
}
```

Restart OpenCode. The plugin registers the Hyperpowers skills directory and
injects the `using-hyperpowers` bootstrap at session start.

Verify by asking: "Tell me about Hyperpowers".

## Usage

Use OpenCode's native `skill` tool:

```text
use skill tool to list skills
use skill tool to load hyperpowers/brainstorming
```

## Tool Mapping

When skills reference Claude Code tools:

- `TodoWrite` -> `todowrite`
- `Task` with subagents -> OpenCode subagent syntax
- `Skill` tool -> OpenCode's native `skill` tool
- File operations -> your native tools
