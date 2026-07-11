# Codex High Autonomy

`codex-high-autonomy` is a lightweight project template for running Codex in a persistent, high-autonomy workflow.

It is designed for projects where Codex should keep moving with minimal supervision, survive context compression, and leave behind enough local state for a newly attached agent to continue without rebuilding everything from scratch.

## What This Template Does

This template gives Codex a small set of Markdown state files that act as a local memory layer:

- `agents.md` defines the operating rules
- `tasks.md` tracks future, current, and completed work
- `latestStatus.md` stores the detailed current status
- `lastsPoints.md` stores a compact recovery summary
- `stop.md` records user interruptions or redirects

Together, these files help Codex:

- keep working in loops until a task is actually finished
- preserve key information before context compression
- resume work after a new thread or a lost context window
- expose progress clearly to the user
- keep important milestones visible in Git history

## How To Use

1. Copy the contents of [`template/`](./template/) into your project or into a dedicated task folder.
2. Update `agents.md` first so it reflects your project rules, language preferences, and task structure.
3. Keep `tasks.md`, `latestStatus.md`, and `lastsPoints.md` updated as the work progresses.
4. Record real stop or redirect events in `stop.md`.
5. Commit important milestones to Git with clear messages.

## Important For Codex

Codex should always look at `agents.md` first.

When a new Codex agent joins the project, or when context has been compressed or lost, `agents.md` should be treated as the main source of truth. It should tell the agent:

- what kind of autonomy is expected
- which files must be read next
- which language should be used for the user
- which language may be used for internal notes
- how state should be preserved between long-running task cycles

## Repository Structure

```text
template/
  agents.md
  tasks.md
  lastsPoints.md
  latestStatus.md
  stop.md
```

## Philosophy

This repository is intentionally simple.

It does not try to replace issue trackers, task managers, or full orchestration systems. Instead, it gives Codex a minimal, file-based operating layer that is easy to understand, easy to version, and easy to recover after context loss.
