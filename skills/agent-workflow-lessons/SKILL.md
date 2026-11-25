---
name: agent-workflow-lessons
description: General working rules for agent-driven dev work — request interpretation, debugging order, secrets handling, delegation, external actions, environment pitfalls, verification. Use at the start of any multi-step dev, deploy or infra task.
---

# Agent workflow — lessons learned

## Understand the request
- Requests may come from voice dictation or fast typing: names get mangled (a product or domain spelled several ways, a negation that is really an imperative). Resolve against known project names; state the assumption in one line instead of asking.
- Answer in the user's language; keep code, commits and UI copy in the language the project already uses unless told otherwise.
- Ambiguous product/tool name: confirm on the official source before any server-side action. Wrong product installed = announce it, remove everything created (containers, volumes, dirs), restart clean.
- Examples given by the user express a criterion, not a category: rephrase the abstract rule ("useful, relevant") before recording it as a durable preference.
- "Check yourself" / "retry": run the read-only checks and report facts; never re-ask an unanswered question in a loop.
- Never invent legal or identity data (company id, address): ask, or leave the field empty.

## Cancel / rollback
- On "cancel"/"undo": stop subagents and processes at once, `git restore` tracked files, delete created files, reinstall from the restored lockfile, verify, then list what was removed, what stays, what was never pushed. No retry.
- Ask feasibility questions before implementing; no CI run, push or release before the user validates the direction.
- Before removing or moving a dir or submodule: `git status --ignored`, back up untracked files, compare content after the move, grep old references.

## Debug infrastructure before code
- "Event X doesn't appear" → check the delivery path first: is the tunnel/webhook endpoint reachable, are credentials present, did a recent cleanup delete secrets? Logs + one `curl` beat reading code.
- After a "wipe everything", list what went with it (tunnels, DNS names, .env, OAuth apps) and warn before the user hits it.

## Secrets
- Never ask the user to paste secrets in chat. Give `read -rsp` / the platform CLI's hidden-prompt command (e.g. `wrangler secret put`), store under a gitignored `.secrets/`, `chmod 600`.
- Generated secrets: write in the scratchpad with `umask 077`, upload, delete.
- Grep config output and redact (`sed 's/SECRET=.*/<set>/'`) before printing.

## Before claiming a limitation
- Check the tool's current docs or search ("does X have a CLI for Y in <year>?"). New official tools supersede memory.

## External / destructive actions
- Inspect before deleting (DNS records, branches, files); show what you'll delete; get one confirmation per new kind of change. Approval for one change does not cover the next.
- Deploying, pushing, DNS and WAF changes are outward-facing: do them only when asked, then verify live (HTTP codes, DNS-over-HTTPS, headers).
- Login flows the agent can't finish (OAuth in a browser): start with `--browser=false` in background, hand the URL to the user, then poll.
- Stop background processes you started once they're useless (tunnels, dev servers); never `pkill -f` patterns that match your own shell — use PIDs.
- Before commit/push/repo creation: check `git config user.name/user.email` (noreply if required) and `gh auth status` (right account, right env WSL/Windows). Stage by explicit path, never a whole folder; review `git status` for unexpected files.
- When asked to "commit": run `git status`/`git log` first, another session may already have committed.
- Push rejected: fetch + rebase, never force. If an update pulls from the remote, push before saying "update now".
- Permission/classifier refusal: don't work around it; explain, ask, relaunch after an explicit yes. Never loosen your own permissions.
- Rebuilding a container/service: restart via its supervisor (pm2, systemd) so it keeps its env; announce downtime risk.
- Don't poll a production site with curl bursts (bot challenge 403); use the platform CLI's deployment status.

## Windows + WSL specifics (when applicable)
- Packaged desktop apps (MSIX) can virtualize `%LOCALAPPDATA%` for commands it launches → Windows installs may have to be run by the user in their own PowerShell.
- Windows user PATH: write via registry as `REG_EXPAND_SZ` (`DoNotExpandEnvironmentNames`), never `SetEnvironmentVariable` (breaks `%VAR%` entries).
- WSL may see only IPv4 while the Windows browser uses IPv6.
- Test installers in the user's real environment (new terminal, persistent PATH); an open terminal keeps the old env, so close all windows. A stale binary earlier in PATH can shadow the new one (`command -v`). After a winget install the session PATH is stale: relaunch the terminal.
- PowerShell scripts meant for both 5.1 and 7: ASCII only, no hard-coded user path (`$HOME`, env vars); `.sh` files keep LF endings. Never overwrite an existing user config file without consent; make scripts idempotent.
- Never delete/recreate a WSL distro that exists: it holds user data. Enabling features (WSL, Sandbox) may need admin + reboot, to be done by the user.
- Untrusted binary or irreversible test: run it in Windows Sandbox (network off, input read-only), read the report, then decide on the host.
- Bash heredocs/awk/sed may silently eat `\` and `$` or write nothing: prefer Write/Edit or a Python script, then check the file exists and is non-empty.
- Text replacements: detect CRLF/LF and accents first; script with `assert s.count(old)==1` so a miss fails loudly; absolute paths in batch scripts.
- Foreground `sleep` may be blocked: use notifications or a monitor, not polling loops.
- Missing system lib without sudo: `apt-get download` + extract to `~/.local` + `LD_LIBRARY_PATH` recorded in an env.sh.
- Driving a Windows app from WSL: `wslpath -w` paths, background/factory-startup flags, never touch the user's open instance.
- A `--dry-run` flag you just wrote is unproven: check it covers sub-scripts on a harmless op before trusting it.

## Delegation
- Orchestrate with the main model; delegate big, well-specified builds to subagents (cheaper model) with: goal, validated architecture decisions, hard contracts to keep, deliverables, "don't commit/deploy", and "report unverified points honestly".
- Run the review gates yourself after delegation (lint, typecheck, tests) and a security review on auth/token changes before committing.

## UX for non-dev audiences
- Guide step by step, labels over paragraphs, never hide important items behind disclosures, mark optional with a badge, visual feedback for filled/missing fields.

## Reporting
- Separate "verified" from "not verified" every time. Exact commands only if they ran. Short tables over walls of text.
- Label each claim measured, inferred or not established; requalify honestly when challenged. Don't list known artifacts as "to fix".
- Performance: control the bench first (visible tab, refresh cap, machine load), measure a no-render baseline, A/B with stash; never state an untested cause.
- If an earlier suggestion contradicts an agreed rule, say so and fix it.
