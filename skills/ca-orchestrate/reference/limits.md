---
covers: platform limits and non-obvious rules to design the machinery around
---

# Limits

Every agent has its own limits on delegation. Before a plan depends on one, check it against the agent you are running in. If you cannot confirm a limit, design as if it is lower.

## What to check

- **Can it spawn subagents at all?** If not, use separate sessions or inline passes.
- **How deep can subagents nest?** A subagent at the bottom level may not get the tool to spawn more.
- **How many agents can run at once?** Parallel work beyond this runs in sequence.
- **Can a multi-agent workflow call another one?** If not, run several in sequence from the main session and read each result before the next.

## Example: Claude Code

Verified in June 2026. Confirm against the live harness if a plan pushes a limit.

| Limit                           | Value                                                |
| ------------------------------- | ---------------------------------------------------- |
| Subagent nesting depth          | 5 levels (a depth-5 agent gets no Agent tool)        |
| Workflow agents — concurrent    | 16                                                   |
| Workflow agents — total per run | 1000                                                 |
| Workflow calling a workflow     | not allowed (one level)                              |
| Workflow-spawned agents         | are leaves — cannot spawn subagents or call Workflow |

## Rules that are easy to miss

- **A multi-agent workflow needs opt-in.** An approved plan that names a workflow is the opt-in. Never launch one the user did not approve.
- **Plan mode vs. this plan.** If the agent has a plan mode and it is in play, it settles _what_ to change. This plan covers only the machinery and the team; do not run a second approval on the solution.
- **Send parallel agent calls together.** Most agents run delegated calls in parallel only when they go out in one message; otherwise they run in sequence.
