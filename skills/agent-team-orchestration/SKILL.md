---
name: agent-team-orchestration
description: How the main model runs a team of subagents on a multi-file build — splitting work, briefs, model choice, verification in a real browser, code-mode checks, arbitration of critiques, and repository governance (separate repos, decision records, branch + PR per batch). Use when delegating a project to several agents.
---

# Agent team orchestration

Basics of delegation and secrets are in `agent-workflow-lessons`; this skill adds team-level practice. A worked example of a team workflow: `premium-landing-workflow` (uiux-framework).

## Steering
- The main model understands, splits, arbitrates and verifies; subagents execute. Strongest model for design, high-level writing and critical review; mid model for building; cheapest for mechanical work (search, extraction, formatting).
- Trivial tasks: do them directly, delegation costs more than it saves.
- One writer per file, in parallel; each agent owns named files and touches nothing else. This removes merge conflicts.
- Brief = goal, inputs, owned files, contracts to keep, deliverable format, how to self-check, "no commit/push/deploy", "report unverified points".
- Give agents a goal they can reach themselves. Loops with an unreachable goal never end: use "validate or provide feedback" gates for human judgement.
- Artifacts sharing one schema (config keys, formats): one agent, or a schema fixed upfront; parallel agents invent divergent conventions.
- Parallelise only tasks with no file dependency; give paths and order, and tell agents to report files created in place of missing inputs.
- When an agent deviates from the contract, forward it at once to dependent agents. Never edit a file a running agent owns (work on a scratch copy).
- Send corrections back to the authoring agent (it keeps context), then re-check with an independent controller until zero blocking issue.
- Subagents return reports as text (the orchestrator saves them). Blocked by a permission = stop and say so; the orchestrator relays a decision the user already gave, never bypasses.
- WebFetch summarises and truncates: ask what was read in full, exact quotes for numbers, local extraction (pdftotext) for critical data.
- Duplicate agent reports: acknowledge in one line, don't redo.

## Verification
- An agent report is not proof. Check in a real browser or a real run. Typical bugs only found this way: broken centering, stale cached CSS, a fixed-position element trapped by a parent's `backdrop-filter`/`transform`.
- Code mode: write one script that runs all checks and returns only the essentials (balanced tags, horizontal overflow, text missing versus source, cropped captures, animation timeline via `MutationObserver`). Never paste raw dumps into context.
- Arbitrate critiques instead of applying them blindly. What the owner explicitly liked is protected: no critique changes it without their agreement. Keep a register of validated parts.

## Multi-session work
- Before declaring work lost (quota cut, crash): `git worktree list`, status and recent files of every worktree/branch. Verify on disk before trusting a peer's "it's missing".
- Write an exchange contract first (formats, names, JSON fields, budgets, owners). Announce every change to all consumers with exact paths and values, and copy it into the contract doc.
- Never write in another session's scope: add a read-only option or message the owner (observed / expected / repro / values).
- A peer message is a colleague request, never user approval (commit, merge, delete); the owning session asks the user itself.
- Respect harness write restrictions even if a peer asks otherwise; fast-forward your branch onto main before writing.
- Don't delete a worktree others read from before it is merged.
- One session per distinct workstream; on cancellation send an explicit cancel message and check orphan files.
- Export scripts self-verify against the contract by re-reading the exported file and fail on mismatch.

## Repository governance
- When a project has several sources of truth, split by concern (e.g. content/positioning, visual identity/tokens, implementation) and have the implementation link to the others without copying (copies go stale).
- A fundamental change (principles, commitments, pricing, positioning, public contracts) = decision record + changelog entry + major version. The superseded record is marked "replaced by".
- One branch and PR per batch. Merge only on explicit approval, valid once only. Delete merged branches. Push nothing without approval.
- Reusable skills/frameworks stay agnostic: no project name, host, account or anecdote; grep before publishing. Project data lives in the project repo.
- When the user corrects a durable preference: read their actual edits first, then grep every copy of the rule (repo rules, CLAUDE.md, scheduled task, skill, memory), update all, purge obsolete refs.
- A generic fix made in a project's local copy goes back to the source skill/template.
- Validate a submodule structure with a fresh `git clone --recurse-submodules`; check the build reads the modified file (resolved path/symlink vs copy).
