# Dev Framework

Reusable [Agent Skills](https://code.claude.com/docs/en/skills) distilled from real development sessions with Claude Code: rules, checklists and decision guides to make the next project go faster and safer.

This repo is one theme of the [framework](https://github.com/chawkitariq/framework) family:

- [dev-framework](https://github.com/chawkitariq/dev-framework) (this repo): general development workflow and technical practice
- [uiux-framework](https://github.com/chawkitariq/uiux-framework): design, copy, motion and premium landing pages
- [ops-framework](https://github.com/chawkitariq/ops-framework): hosting, Cloudflare and infrastructure operations

## Skills

| Skill | Use it when |
|---|---|
| [`agent-workflow-lessons`](skills/agent-workflow-lessons/SKILL.md) | Starting any multi-step dev, deploy or infra task: debugging order, secrets handling, external actions, delegation, verification |
| [`agent-team-orchestration`](skills/agent-team-orchestration/SKILL.md) | Running several subagents on one build: splitting, briefs, model choice, verification in a real browser, repo governance |
| [`technical-seo`](skills/technical-seo/SKILL.md) | Preparing a static site for release or auditing it: meta, social cards, structured data, sitemap, performance, control script |

## Agents

| Agent | Model | Role |
|---|---|---|
| [`seo-specialist`](agents/seo-specialist.md) | sonnet | Implements and audits technical SEO using the `technical-seo` skill |

## Install (Claude Code)

```bash
git clone https://github.com/chawkitariq/dev-framework.git
cp -r dev-framework/skills/* ~/.claude/skills/
cp dev-framework/agents/*.md ~/.claude/agents/
```

Use `<project>/.claude/skills/` and `<project>/.claude/agents/` to scope them to one project.

## License

MIT
