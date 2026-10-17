---
name: plan-master
description: "Master command for turning an idea, feature or project into a plan and carrying it out: shaping the idea, writing the plan, choosing inline, subagent or parallel execution, git branches and worktrees, finishing with a PR, handoff or standup. Interviews the user, shows a brief, then runs the planning and execution skills together. Use only when the user types /plan-master or asks for the plan master."
---

# Plan Master

## How this master runs

Follow these phases in order. Do not skip the brief.

### Phase 0: Look before asking
Scan what is already known: the user's message, open files, the repo (stack, configs, README, DESIGN.md, brand files, tests, git state), and memory. Any intake question already answered here is skipped.
If `<project>/.claude/project-brief.md` exists, read it first and treat its answers as given. If it starts with `Run by:` (for example `/app-master` or `/audit-master`), this master is one stage of a bigger run: ask only questions the brief does not cover, use its Visuals and Teamwork answers, present your Phase 4 brief as that stage's checkpoint (one approval, not two), and update the brief's Stages table when done. If the brief says `Mode: report only`, change no code: append every finding to the brief's Findings table (severity, effort, area, file:line, finding, suggested fix) and hand back. Tell the user in one line what you detected ("Detected: Next.js + Tailwind, existing site, no brand kit").

### Phase 1: Intake interview
- Ask the questions under **Intake** below, one round at a time.
- In Claude Code, use the AskUserQuestion tool: at most 4 questions per round, 2 to 4 options each. Put the option you would recommend first and add "(Recommended)" to its label. The user can always type their own answer.
- Without that tool (claude.ai, other clients), ask the round in chat as a numbered list with lettered options, and wait for the reply.
- Skip questions that Phase 0 or an earlier answer already settled. Add a question whenever an answer opens a decision that is the user's to judge. Asking more is better than guessing.
- Treat links and file paths in free-text answers as references to read.
- Unanswered or "you decide" questions get a sensible default. List every default in the brief.

### Phase 1b: Visuals (always ask, just before teamwork mode)
Ask: "How much visual aid do you want?"
- **Full (Recommended)**: a picture at the brief, at every decision, and in the report.
- **Key moments only**: the brief and the report.
- **Text only**: no visuals.

If the answer is Full or Key moments, also ask: "Show a live progress board while I work? It redraws after every step, so it costs extra tokens (roughly 10 to 25% more on long runs)." Options: **No (Recommended)** · **Yes**.

### Phase 2: Teamwork mode (always ask, as the last intake question)
Ask: "How should the skills work together on this?" with these options, explained in plain words:
- **Solo (quick)**: one skill, picked as the best fit, does the whole job. Fastest and cheapest. Good for small or clear tasks.
- **Relay**: the chosen skills take turns in one line, each handing its result to the next (for example: research, then design, then review). Medium cost. Good for most tasks.
- **Panel + judge**: 2 to 4 skills each work out their own approach at the same time (as separate agents). A judge compares them side by side, picks the best or merges the best parts, and explains why. Then one builder carries it out. Highest quality, about 2 to 4 times the cost. Good for big, high-stakes or taste-driven work.

### Phase 3: Research (only if the user asked for it, or the task needs facts you do not have)
Search trusted, current sources. Collect findings with links. For taste-driven work, gather 5 to 8 concrete references and say what each one is for.

### Phase 4: Brief, then wait
Show a one-screen brief:
1. Goal, audience, scope, constraints (from the answers)
2. Defaults you chose for skipped questions
3. Research findings and references (with links), if any
4. Child skills picked, their order, and the teamwork mode
5. Plan: numbered steps, what will be created or changed, how it will be verified
Then ask for "go" or changes. Do not build anything before the user says go.

### Phase 5: Run the child skills
- **Using a child skill:** if it is installed (it appears in your skill or command list, possibly with a plugin prefix such as `plugin:name`), invoke it with the Skill tool so the newest version is used. Otherwise read the bundled copy at `children/<name>/CHILD.md` inside this skill's folder and follow it; its helper files sit next to it. Children marked "safe copy" below are always run from the bundled copy, and every child listed under **Safety overrides** follows that rule however it was installed. If a child is neither installed nor bundled, show its install line from **Where child skills come from** and ask whether to continue without it. If a child needs a tool or account that is missing, say so in one line and use the closest alternative.
- **Bundled agent prompts:** some children carry `agents/*.md`. To use one, start a subagent (general-purpose) whose prompt is that file's body plus the task.
- **Solo:** run the single best child skill.
- **Relay:** run the children in the order from the brief. Pass each one a short summary of the brief plus the previous step's output.
- **Panel + judge:** start one agent per panel skill in parallel. Each gets the full brief and the instruction to propose (not build) an approach in under 400 words. Then judge: score each proposal against the user's answers (fit to goal, fit to stated taste or references, risk, effort), show the scores in a small table, and pick or merge. If the call is close or taste-driven, ask the user to pick. Then build with the winning approach.
- Whenever a new decision comes up that is the user's to judge, stop and ask it. Never take irreversible or outward-facing steps (push, publish, delete, post, send, pay) without an explicit yes at that moment.

### Phase 6: Verify
Check the result against the brief with real evidence (run it, test it, screenshot it, re-read it). Fix what fails. Anything that could not be checked goes under "Not verified".

### Phase 7: Report
Use the **Output** template below. Keep it short. End with at most one suggested next step.

## How visuals work (unless the user chose Text only)
Each master lists its own visuals under **Visual aids** further down. Show them at these moments:
- **Brief** (Phase 4): a picture of what will change before the user says go: files or areas touched, the flow, a before/after sketch.
- **Decisions** (Phase 5, Full only): whenever the user must pick, show the options side by side (directions, approaches, panel proposals with the judge's scores).
- **Report** (Phase 7): proof of what changed: before/after screenshots, a diff map, charts of test or metric results.
- **Live board** (only if the user said yes): a compact board of steps (done, running, next) redrawn after each step.

**Show each visual in two places:**
1. **Inline in the chat.** Use the best tool this client offers, in this order: an inline visual/widget tool (for example `show_widget`; load its guide first if it has one) → an Artifact → a Mermaid diagram or a small Markdown table in the reply.
2. **One visual report page per run.** Copy `visuals/report-template.html` from this skill's folder to `<project>/.claude/master-runs/<YYYY-MM-DD>-<master>-<short-task>.html`, fill its sections as the run goes (brief, decisions, report), and keep appending. Open it for the user (browser pane, Artifact, or give the path). On claude.ai, publish it as an Artifact instead of a file. Never commit this folder unless the user asks.

**Rules for every visual:**
- Real data only: numbers, files and screenshots come from the repo, the diff, test output or the browser. Anything illustrative is labelled "sketch".
- The title states the takeaway ("3 files change, no public API touched"), so it reads in 5 seconds.
- Use this master's color for emphasis, gray for everything else, red only for problems and green only for passes.
- Every visual has a text equivalent next to it (one line), so nothing is lost in a text-only client.
- Screenshots: take them with the browser tools at desktop (1440px) and mobile (390px) widths when the work touches a UI.

## Intake

**Round 1: what and why**
1. `Goal` | What are we building? | New user-facing feature · Internal tooling · Spike / prototype (throwaway) · Fix or improve something existing
2. `Clarity` | How defined is it? | One-liner (needs shaping) · Rough notes · Written spec / PRD · An existing plan file (just execute)
3. `Size` | How big? | Under an hour, 1 to 2 files · Half a day · Multi-day, many modules · Unknown (investigate first)
4. `Research` | Research first? | Codebase only · Web and docs first · Both · Skip

**Round 2: limits**
5. `Non-goals` | Anything explicitly out of scope? | Yes, I'll list it · Keep it minimal, nothing extra · You propose, I approve · None
6. `Constraints` | Hard constraints? | No new dependencies · Must stay backward compatible · Performance / size budget · None
7. `Risk` | Risk tolerance? | Low: small steps, review each · Medium: checkpoints · High: move fast
8. `Tests` | Testing? | Tests first (TDD) · Tests alongside · Manual check only · Follow the repo's habit

**Round 3: delivery**
9. `Git` | Where should work happen? | Current branch · New branch · Isolated worktree · No git
10. `Finish` | When done? | Open a PR · Merge locally · Leave the branch for me · Commit nothing
11. `Deadline` | Time pressure? | Today · This week · None
12. `Done` | Definition of done? | Tests green and verified · Plus docs / changelog · Plus a handoff note · Plan only, don't build

Then ask Teamwork mode (see protocol).

## Child skills and routing
| Child | Use when |
|---|---|
| make-plan | Clarity = one-liner or notes: shape the idea first. |
| writing-plans | Always, unless an existing plan file is given. |
| feature-dev | A feature that needs codebase exploration and architecture options; its code-explorer, code-architect and code-reviewer agents make a good panel. |
| using-git-worktrees | Git = worktree. |
| subagent-driven-development | Independent tasks and subagents available. |
| dispatching-parallel-agents | 2+ tasks with no shared state. |
| executing-plans | Small jobs, or no subagents: run inline. |
| do | The plan file uses phased `do` format. |
| test-driven-development | Tests = TDD. |
| finishing-a-development-branch | Finish = PR or merge. |
| handoff | Done includes a handoff note, or work stops mid-way. |
| standup | The user asks for status, or a daily summary. |
| build | Autopilot: the user wants it hands-off; runs make-plan, do and babysit in one go. |
| build-and-release | Autopilot through to release; stops for a yes before merging and before releasing. |

Rules: Done = plan only: stop after the plan and show it. Size = Unknown: investigate before planning. Ask before pushing or opening a PR.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `flow` | The plan as a step flow: tasks in order, which run in parallel |
| Brief | `diagram` | File map: folders and files each task will touch |
| Decisions | `options` | Architecture options side by side (from feature-dev's architect), trade-offs and scores |
| Report | `timeline` | What got done, in order, with commits and branch/PR state |
| Live | `board` | Task board: done, running, next |

## Output
1. **Goal / non-goals**
2. **Plan**: path or summary, number of tasks
3. **Done**: task, commit or branch
4. **Verification**: commands run and results
5. **Git**: branch or worktree, PR link or merge state
6. **Open questions / risks**, **next step or handoff**

## Child skills
Not bundled: use the installed versions (see sources below).
- `make-plan`
- `writing-plans`
- `feature-dev`
- `using-git-worktrees`
- `subagent-driven-development`
- `dispatching-parallel-agents`
- `executing-plans`
- `do`
- `test-driven-development`
- `finishing-a-development-branch`
- `handoff`
- `standup`
- `build`
- `build-and-release`


## Safety overrides
These rules apply to these children whether they come bundled or installed:
- `build-and-release`: stop before merging the PR and before releasing; each needs a fresh yes.


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| claude-mem plugin | `make-plan`, `do`, `handoff`, `standup`, `build`, `build-and-release` | `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem@thedotmack` |
| Superpowers (obra) | `writing-plans`, `using-git-worktrees`, `subagent-driven-development`, `dispatching-parallel-agents`, `executing-plans`, `test-driven-development`, `finishing-a-development-branch` | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
| feature-dev plugin (Anthropic marketplace) | `feature-dev` | `/plugin install feature-dev@claude-plugins-official` |
