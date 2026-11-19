# memory-upkeep

A Claude Code skill for keeping long-lived project memory accurate, compact, and genuinely useful across sessions.

## The problem

On a long-running project, context gets lost between sessions. The usual fix — writing everything down — creates a second problem: the memory file grows until every new session pays a large load cost just to start, and much of what it contains has quietly gone stale.

## The core principle

Memory files should only hold what **cannot be re-derived from the repo**.

Commit logs, deployment versions, and day-by-day development notes already live in git and in your handover docs. Duplicating them makes every session more expensive and the file more likely to be wrong. What belongs in memory is the reasoning behind a rule, the trap you already fell into once, how real users actually reacted, and the current state of play.

## What the skill covers

- A layered file structure — a short auto-loaded index pointing to topic files that are read only on demand
- A step-by-step upkeep routine: survey the current state first, audit for gaps, record incidents before compressing anything, then archive what has settled
- Guidance on splitting a new branch of work into its own file, and on deciding whether a request is a small edit or a new thread
- How to read `/context` properly — distinguishing conversation cost, loaded memory cost, and fixed MCP tool cost
- Environment-specific rules: when pushing is optional, and when forgetting to push wipes your memory entirely
- Working safely across several parallel sessions

## Installation

Place `SKILL.md` at:

```
~/.claude/skills/memory-upkeep/SKILL.md          # available in every project
<project>/.claude/skills/memory-upkeep/SKILL.md  # scoped to one project
```

The skill triggers on topics like wrapping up a session, context usage, or picking an old project back up. You don't need to invoke it by name.

## Origin

This grew out of running a production system through launch and incident response. The rules here are the ones that turned out to matter, not a theoretical best-practice list.

## Adapting it

Several judgement calls here are specific to how one project ran. Treat them as a starting point and rewrite them for your own situation rather than adopting them wholesale.

The skill body is written in Traditional Chinese, including its trigger description — if you work in another language, translate the `description` field in the frontmatter so it matches how you actually phrase these requests.

## License

MIT — see [LICENSE](LICENSE).
