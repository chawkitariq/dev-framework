---
name: agent-team-orchestration
description: How the main model runs a team of subagents on a multi-file build — splitting work, briefs, model choice, verification in a real browser, code-mode checks, arbitration of critiques, and repository governance (separate repos, decision records, branch + PR per batch). Use when delegating a project to several agents.
---

# Agent team orchestration

Basics of delegation and secrets are in `agent-workflow-lessons`; this skill adds team-level practice. Example application: `premium-landing-workflow`.

## Steering
- The main model understands, splits, arbitrates and verifies; subagents execute. Strongest model for design, high-level writing and critical review; mid model for building; cheapest for mechanical work (search, extraction, formatting).
- Trivial tasks: do them directly, delegation costs more than it saves.
- One writer per file, in parallel; each agent owns named files and touches nothing else. This removes merge conflicts.
- Brief = goal, inputs, owned files, contracts to keep, deliverable format, how to self-check, "no commit/push/deploy", "report unverified points".
- Give agents a goal they can reach themselves. Loops with an unreachable goal never end: use "validate or provide feedback" gates for human judgement.

## Verification
- An agent report is not proof. Check in a real browser or a real run. Bugs found this way: broken centering, stale cached CSS, a mobile menu trapped in a parent's height by `backdrop-filter`/`transform`.
- Code mode: write one script that runs all checks and returns only the essentials (balanced tags, horizontal overflow, text missing versus source, cropped captures, animation timeline via `MutationObserver`). Never paste raw dumps into context.
- Arbitrate critiques instead of applying them blindly. What the owner explicitly liked is protected: no critique changes it without their agreement. Keep a register of validated parts.

## Repository governance
- Split by concern: a "what to say" repo (content, positioning), a "look" repo (tokens, logos), and an implementation repo that links to both without copying (copies go stale).
- A fundamental change (principles, commitments, prices, positioning) = decision record + changelog entry + major version. The superseded record is marked "replaced by".
- One branch and PR per batch. Merge only on explicit approval, valid once only. Delete merged branches. Push nothing without approval.
