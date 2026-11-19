# Project memory upkeep

A tool-neutral method for keeping a coding agent's long-term project memory accurate, compact, and worth loading. The Claude Code implementation lives in [`SKILL.md`](SKILL.md); this file is the method itself, usable by any agent that reads an instruction file.

## The core principle

**Only record what cannot be re-derived from the repo.**

Commit logs, deployment versions, and day-by-day development notes already live in git and in the project's own docs. Copying them into a memory file makes every session more expensive to start and makes the file more likely to be wrong.

| Record this | Leave this out |
|---|---|
| Why a rule exists — the reasoning behind a design choice or a specific number | Day-by-day development logs |
| Traps already fallen into: ordering constraints, schema landmines | Commit hashes, deployment version history |
| How real users actually reacted, in their own words | Structure that is obvious from reading the code |
| Implicit team and operational knowledge; single points of failure | Test reports and other docs that already exist |
| Current state, open items, things awaiting verification | Finished work that will not be revisited (→ archive) |

## Structure

A short index file that loads on every session, pointing to topic files that are read only when needed:

```
<index file>          loaded every session — a few lines
├─ <topic>_current    current state, open items, awaiting verification
├─ <topic>_feedback   what users actually said and did
├─ <topic>_ops        team and operational knowledge
├─ <topic>_<thread>   each major new thread gets its own file
└─ archive_<period>   settled history
```

Every index line must say *what is in that file*, not just name it. The index is how a fresh session decides which one file to open instead of loading everything.

## The upkeep routine

**1. Survey before changing anything.** Which memory files exist, how large, last updated? Is this environment persistent, or rebuilt from scratch each session? Is another session editing the same files right now? Skipping this step is how you overwrite someone else's work or build a structure that fights the existing one.

**2. Audit for gaps — list them before writing.** What has happened since the last update that was never recorded: finished work, fixed bugs, schema or deployment changes, settled decisions, user feedback, incidents. Conclusions reached out loud but never written down are the most commonly lost category.

**3. Record incidents before compressing anything.** Details fade; compression can wait. Capture what happened, the root cause, what was done at the time, whether it was actually fixed or merely contained, how to prevent a recurrence, and — most valuable — which assumptions it proved wrong. Operational lessons (a miscommunication, the same problem "fixed" three times while meaning three different things) matter as much as technical ones.

**4. Route each item to the right file.** Don't let everything accumulate in one. When a new thread gets its own file, leave a short pointer behind saying where it went, so nobody later wonders whether it was lost.

**5. Compress and archive.** Settled material moves to archive; anything duplicated from the repo gets deleted; open and unverified items stay separate so they aren't buried in history. Timing matters: don't compress while facts can still be overturned — wait until things have settled.

**6. Sync.** If memory lives in a workspace that is rebuilt each session, committing it is not housekeeping — it is the only reason it survives. Say so at the top of the index file. If the repository is public, operational detail, user quotes, and anything about specific people must not be written there at all. Calibrate what you record to where it will be stored.

**7. Report what changed.** Sizes before and after, what was added, and anything the user should know about but has not addressed yet.

## Returning after time away

When someone comes back to a project and has lost the thread, don't make them guess. Read the index and the relevant topic files, then give them a current-state summary: where this work left off, known traps, unfinished items, anything awaiting verification. They decide the next step after reading it.

## Small edit, new thread, or unsure

- **Small edit** — append to the existing topic file's open-items list; no new file.
- **New thread** — its own file, one new index line.
- **Unsure** — read the existing memory first and judge how tightly it couples to what's already there. Cheaper than guessing and moving files later.

## Context budget

Distinguish three different costs before trying to reduce any of them:

1. **Conversation** — accumulates as you work; clearing the session resets it.
2. **Loaded memory** — normally just the index. Topic files are read on demand and *should not* show up as always-loaded; their absence from that list doesn't mean they weren't written.
3. **Fixed tooling** — loaded at session start regardless of how long you work. Clearing the session does nothing for this; removing unused tooling does.

Wrap up while roughly a third of the budget remains. Past that, automatic compaction decides what survives — doing it deliberately keeps that choice yours.

## Parallel sessions

People often work in several windows at once. Before editing, check whether another session is touching the same files. After a split or a large restructure, tell the other session what moved. State at the start of each session which area it owns.
