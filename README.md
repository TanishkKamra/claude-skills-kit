# Master commands for Claude Code

14 commands that interview you (multiple-choice), show a brief, wait for your "go", then run the right skills together and report back with visuals.

## Install

In Claude Code:

```
/plugin marketplace add TanishkKamra/claude-skills-kit
/plugin install masters@claude-skills-kit
```

Restart Claude Code. Updates arrive with `/plugin marketplace update claude-skills-kit`.

This repo is private: you need access on GitHub, and Claude Code uses your GitHub login (`gh auth login`) to fetch it.

## Commands

| Command | What it does |
|---|---|
| `/masters:ui-master` | Design, redesign, polish or review any interface |
| `/masters:visuals-master` | Brand kits, mockup images, posters, image-to-code |
| `/masters:plan-master` | Idea → plan → build, branches, PRs, handoffs |
| `/masters:refactor-master` | Safe fixes, refactors, migrations, minimal builds |
| `/masters:release-master` | Commits, PRs, branch cleanup, issues, versions, releases |
| `/masters:ops-master` | Production readiness: reliability, speed, logs, deploys |
| `/masters:debug-master` | Bugs, tests, verification, browser QA, break-it testing |
| `/masters:review-master` | Code review, security audits, over-engineering audits |
| `/masters:codemap-master` | Understand a codebase, architecture maps |
| `/masters:research-master` | Web, social, papers, docs and finance research |
| `/masters:docs-master` | Slide decks, documents, proposals, comms |
| `/masters:memory-master` | Past work, digests, usage reports |
| `/masters:style-master` | Terse replies, token saving, commit style |
| `/masters:builder-master` | New skills, plugins, hooks, MCP servers |

Also included: `torture-test` (adversarial QA of your projects) and `ui-polish` (UI audit and fix).

## Child skills

The masters orchestrate other people's skills. They are **not** copied here; each master lists, under "Where child skills come from", the exact install line for every child it uses, and tells you when one is missing. Most come from:

- `obra/superpowers-marketplace` (planning, TDD, debugging, review)
- `Leonxlnx/taste-skill` (frontend design taste)
- `anthropics/skills` (docs, themes, canvas, web app testing)
- the official Anthropic plugin marketplace (code review, feature-dev, plugin-dev, research tools)
- `JuliusBrussee/caveman`, `DietrichGebert/ponytail`, `pbakaus/impeccable`, `thedotmack/claude-mem`

## Every run

Look → interview (incl. visuals level and teamwork mode: Solo / Relay / Panel + judge) → optional research → brief and wait → run → verify → report. Never pushes, publishes, deletes or pays without a fresh yes.
