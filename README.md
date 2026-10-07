# skills

Agent skills by codingawayy. Each skill is a folder with a `SKILL.md`, in the open
[Agent Skills](https://agentskills.io) format. They work in Claude Code, Codex, Cursor, GitHub
Copilot, Gemini CLI, and other agents that support the format.

## Install

```bash
npx skills add codingawayy/skills
```

The CLI finds the agents you have installed and copies the skills into the right folder for each.

To install by hand, copy a folder from `skills/` into your agent's skills folder, for example
`~/.claude/skills/` for Claude Code or `~/.codex/skills/` for Codex.

## Skills

| Skill            | What it does                                                                                                                                          |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ca-orchestrate` | Plans how to run a substantial task: a team of contrasting personas, the least machinery that fits, a plan file you approve, and a review gate where each key persona signs off with evidence. |
| `ca-organize`    | Rules for where information lives across files and folders: one home per fact, no file-to-file pointers, one index per folder, and timeless documents. |

## Use

Ask your agent in plain words, for example "orchestrate this task", or call a skill by name, for
example `/ca-orchestrate` in Claude Code.
