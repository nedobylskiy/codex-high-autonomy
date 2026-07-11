# Codex High Autonomy Rules

This file is the primary source of truth for Codex behavior in this project.
If context was compressed, lost, or a new Codex agent is attached to the project, start here.

If you are a new agent, read these files in order:
- `agents.md`
- `.secrets.md` when secrets, credentials, or external integrations may matter
- `tasks.md`
- `latestStatus.md`
- `lastsPoints.md`
- `stop.md` if it exists and contains a real stop record

## Scope

These rules are written specifically for Codex.
If other agents are used in the same repository, they should not override this file unless the user explicitly decides so.

## Language Rules

- User-facing communication must stay in the user's language.
- Internal working notes may switch to a more token-efficient language when helpful.
- The agent may choose a different language for internal context, task notes, and compact summaries if that reduces token usage.
- The selected user language and internal context language must always be recorded in this file.

### Current Language Selection

- User communication language: `<set-user-language-here>`
- Internal context language: `<set-internal-context-language-here>`

When the language choice changes for a task, update this file.

## Goal

This project runs in a high-autonomy mode.
The agent should keep moving the task forward with minimal user intervention until:
- the task is fully solved
- validation, tests, and basic verification are completed
- or a real blocking condition makes further progress impossible

## Core Principles

1. The agent works in a loop and should keep taking the next meaningful step while progress is possible.
2. The agent should not stop at analysis if implementation, testing, or verification can already continue.
3. The agent must preserve important state in local Markdown files so work survives context compression or thread changes.
4. The user may interrupt, redirect, or stop the work at any time. Such events must be recorded.
5. Every major task inside the project should get its own task folder.
6. Important milestones should be captured in Git with clear commit messages.
7. If project secrets are relevant, the project should maintain a `.secrets.md` file and keep it ignored by Git.

## Required Start Sequence For A New Major Task

When a new major task begins, the agent should:

1. Create a dedicated task folder.
2. Ensure `.secrets.md` exists if the task depends on secrets, credentials, or external integrations.
3. Ensure `.secrets.md` is listed in `.gitignore`.
4. Create or update a local `tasks.md` for that task when needed.
5. Record the current goal and next steps in `latestStatus.md`.
6. Maintain `lastsPoints.md` as a compact recovery note for context restoration.

## Required Files

### `agents.md`

Stores the main Codex high-autonomy rules for the project.

### `tasks.md`

Should contain three sections:
- future tasks
- current tasks
- completed tasks

Constraint:
- keep only the most recent 30 completed items

### `lastsPoints.md`

A compact recovery summary containing:
- what is already done
- what is being worked on now
- what should happen next
- active risks, limits, or unfinished checks

This file may and should be overwritten as work evolves.

### `latestStatus.md`

A more detailed operational status than `lastsPoints.md`, including:
- current work
- recent changes
- active problems
- next steps

This file should help both the user and a newly attached agent.

### `stop.md`

If the user stops execution or changes direction, record:
- the reason for the stop or redirection
- the last completed action
- the state in which the work was left

### `.secrets.md`

Stores project-level information about secrets available to the agent.

Rules:
- create it when a project depends on secrets or external credentials
- keep it in `.gitignore`
- use it to document what secrets exist and what they are used for
- do not commit actual secret values to public repositories

## Execution Rules

The agent should continue working until one of these states is reached:
- the task is fully solved
- tests and checks are completed
- an unrecoverable blocking issue is found
- a practical timeout is reached and explicit user instruction is needed to resume

Examples of blocking issues:
- disk space is exhausted
- required tools are unavailable
- task requirements conflict in a way that cannot be resolved reasonably without the user
- an external dependency is unavailable and completion depends on it

## Context Compression Rules

If the agent believes context compression may happen soon, it must update `lastsPoints.md` first.

At minimum, preserve:
- the current goal
- the key completed steps
- the unfinished actions
- the exact point from which work should resume

## Git Rules

1. Every project should be a Git repository.
2. Important milestones should be committed as separate commits.
3. Each commit message should clearly describe what was captured.
4. Git may be used as a history source, a context source, and a rollback point.
5. The agent must not remove or blindly overwrite user changes without explicit user approval.
6. `.secrets.md` must stay ignored by Git unless the user explicitly wants a private-repository workflow that commits it.

## Instructions For A Newly Attached Agent

If you join this project after context loss or in a new thread:

1. Read `agents.md`.
2. Read `.secrets.md` if the task may depend on secrets, credentials, or external services.
3. Read `latestStatus.md`.
4. Read `lastsPoints.md`.
5. Read `tasks.md`.
6. Check `stop.md`.
7. Inspect Git history if recent decisions need to be reconstructed.
8. Resume the active task instead of rebuilding context from scratch.
