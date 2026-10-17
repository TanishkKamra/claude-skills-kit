# Master commands for Claude Code

15 commands that interview you (multiple-choice), show a brief, wait for your "go", then run the right skills together and report back with visuals.

## Before you install

- [Claude Code](https://code.claude.com) installed.
- Access to this repo: it is private, so the owner must add you as a collaborator.
- Logged in to GitHub on your machine, so Claude Code can fetch a private repo:

  ```bash
  gh auth login
  ```

## Install

**One line, in a normal terminal** (not inside Claude Code):

Mac, Linux or Git Bash:

```bash
claude plugin marketplace add TanishkKamra/claude-skills-kit && claude plugin install masters@claude-skills-kit
```

Windows PowerShell:

```powershell
claude plugin marketplace add TanishkKamra/claude-skills-kit; claude plugin install masters@claude-skills-kit
```

**Or inside Claude Code**, one after the other:

```
/plugin marketplace add TanishkKamra/claude-skills-kit
/plugin install masters@claude-skills-kit
```

Restart Claude Code afterwards.

## Use

To build a whole app from scratch, run `/masters:app-master` and answer one interview; it runs the other masters for you, pausing after each stage.

Or type any single master, optionally followed by what you want:

```
/masters:ui-master redesign my landing page so it stops looking AI-made
```

Every run goes the same way:

1. **Look**: reads your message and repo, and skips questions it can already answer.
2. **Interview**: 10–16 multiple-choice questions, up to 4 at a time. The first option is the usual best pick; you can always type your own answer.
3. **Visuals**: pick *Full*, *Key moments only* or *Text only*. A live progress board is optional (costs ~10–25% more tokens).
4. **Teamwork mode**:
   - *Solo*: one best-fit skill does the job. Fastest, cheapest.
   - *Relay*: skills take turns, each handing its result to the next.
   - *Panel + judge*: 2–4 skills propose approaches in parallel, a judge scores them, one builds the winner. Best quality, ~2–4× the cost.
5. **Brief**: a one-screen plan. Nothing is built until you say **go**.
6. **Run → verify → report**: results with real evidence, plus a visual report page saved at `.claude/master-runs/` in your project.

A master never pushes, publishes, deletes, posts or pays without a fresh yes at that moment.

## Commands

| Command | What it does |
|---|---|
| `/masters:app-master` | **Start here for something new**: one interview, then it runs the research, plan, visuals, UI, build, test, security, ops, docs and release masters, with a checkpoint after each stage |
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

Also included: `/masters:torture-test` (adversarial QA of your projects, report only) and `/masters:ui-polish` (UI audit and fix).

## Child skills

The masters orchestrate other people's skills. They are **not** copied here. When a master needs one you don't have, it shows the exact install line and asks whether to continue without it. Each master's full list is under "Where child skills come from" in its `SKILL.md`. The main sources:

| Source | Install |
|---|---|
| Superpowers (planning, TDD, debugging, review) | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
| Taste skills (frontend design) | `/plugin marketplace add Leonxlnx/taste-skill` then `/plugin install taste-skill@taste-skill` |
| Anthropic example skills (docs, themes, canvas, web app testing) | `/plugin marketplace add anthropics/skills` then `/plugin install example-skills@anthropic-agent-skills` |
| Official Anthropic plugins (code review, feature-dev, plugin-dev, research tools) | `/plugin install <name>@claude-plugins-official` |
| caveman, ponytail, impeccable, claude-mem | `JuliusBrussee/caveman`, `DietrichGebert/ponytail`, `pbakaus/impeccable`, `thedotmack/claude-mem` |

## Update

```
/plugin marketplace update claude-skills-kit
```

Then restart Claude Code. This repo is updated automatically when the masters change.

## Uninstall

```
/plugin uninstall masters@claude-skills-kit
/plugin marketplace remove claude-skills-kit
```

## Troubleshooting

- **"Repository not found" when adding the marketplace**: you don't have access yet, or you aren't logged in. Ask the owner for access, then run `gh auth login`.
- **Command not found**: plugin commands are namespaced; type `/masters:` and pick from the list. Restart Claude Code after installing.
- **A master says a child skill is missing**: run the install line it shows, restart, and run the master again. Or answer "continue" to let it use the closest alternative.
