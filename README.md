# Dev Framework

Reusable [Agent Skills](https://code.claude.com/docs/en/skills) distilled from real development sessions with Claude Code: rules, checklists and decision guides to make the next project go faster and safer.

| Skill | Use it when |
|---|---|
| [`agent-workflow-lessons`](skills/agent-workflow-lessons/SKILL.md) | Starting any multi-step dev, deploy or infra task: debugging order, secrets handling, external actions, delegation, verification |
| [`cloudflare-ops`](skills/cloudflare-ops/SKILL.md) | Deploying a Worker to a custom domain, managing DNS/WAF with `cf`, restricting access by IP, auditing a zone's security |
| [`free-hosting-architecture`](skills/free-hosting-architecture/SKILL.md) | Choosing free or cheap hosting, or re-architecting an app for serverless |

## Install (Claude Code)

Copy or symlink the skill folders into `~/.claude/skills/` (all projects) or `<project>/.claude/skills/` (one project):

```bash
git clone https://github.com/chawkitariq/dev-framework.git
ln -s "$PWD/dev-framework/skills/"* ~/.claude/skills/
```

## License

MIT
