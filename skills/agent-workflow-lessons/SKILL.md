---
name: agent-workflow-lessons
description: Working rules learned from long sessions with a voice-dictating user on WSL + Windows — debugging order, secrets handling, delegation, external actions, verification. Use at the start of any multi-step dev, deploy or infra task.
---

# Agent workflow — lessons learned

## Understand the request
- The user dictates by voice: names get mangled (e.g. "Kik" = Kick, a domain spelled three different ways, a negation that is really an imperative). Resolve against known project names; state the assumption in one line instead of asking.
- Answer in the user's language; keep code, commits and UI copy in English unless told otherwise.

## Debug infrastructure before code
- "Message X doesn't appear" → check the delivery path first: is the tunnel/webhook endpoint reachable, are credentials present, did a recent cleanup delete secrets? Logs + one `curl` beat reading code.
- After a "wipe everything", list what went with it (tunnels, DNS names, .env, OAuth apps) and warn before the user hits it.

## Secrets
- Never ask the user to paste secrets in chat. Give `read -rsp` / `wrangler secret put` style hidden prompts, store under a gitignored `.secrets/`, `chmod 600`.
- Generated secrets: write in the scratchpad with `umask 077`, upload, delete.
- Grep config output and redact (`sed 's/SECRET=.*/<set>/'`) before printing.

## Before claiming a limitation
- Check the tool's current docs or search ("does X have a CLI for Y in <year>?"). New official tools (e.g. Cloudflare `cf`) supersede memory.

## External / destructive actions
- Inspect before deleting (DNS records, branches, files); show what you'll delete; get one confirmation per new kind of change. Approval for one change does not cover the next.
- Deploying, pushing, DNS and WAF changes are outward-facing: do them only when asked, then verify live (HTTP codes, DNS-over-HTTPS, headers).
- Login flows the agent can't finish (OAuth in a browser): start with `--browser=false` in background, hand the URL to the user, then poll.
- Stop background processes you started once they're useless (tunnels, dev servers); never `pkill -f` patterns that match your own shell — use PIDs.

## Windows + WSL specifics
- Claude Desktop (MSIX) virtualizes `%LOCALAPPDATA%` for commands it launches → Windows installs must be run by the user in their own PowerShell.
- Windows user PATH: write via registry as `REG_EXPAND_SZ` (`DoNotExpandEnvironmentNames`), never `SetEnvironmentVariable` (breaks `%VAR%` entries).
- WSL sees only IPv4 sometimes while the Windows browser uses IPv6.

## Delegation
- Orchestrate with the main model; delegate big, well-specified builds to subagents (cheaper model) with: goal, validated architecture decisions, hard contracts to keep, deliverables, "don't commit/deploy", and "report unverified points honestly".
- Run the review gates yourself after delegation (lint, typecheck, tests) and a security review on auth/token changes before committing.

## UX for non-dev audiences
- Guide step by step, labels over paragraphs, never hide important items behind disclosures, mark optional with a badge, visual feedback for filled/missing fields.

## Reporting
- Separate "verified" from "not verified" every time. Exact commands only if they ran. Short tables over walls of text.
