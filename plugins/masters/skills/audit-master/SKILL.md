---
name: audit-master
description: "Master command for putting an existing app through everything: understand the codebase, run and extend its tests, break-it testing, security and code review, UI and accessibility review, production readiness, then one ranked fix list where the user picks what to fix, with each fix verified. Runs the other master commands stage by stage (codemap, debug, review, ui, ops, refactor, docs, release) with a checkpoint after each. Use only when the user types /audit-master or asks for the audit master."
---

# Audit Master

Audits an app that already exists by running the other masters in order. Audit stages only report; nothing changes until the user picks fixes from one combined, ranked list.

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
Phase 0 detects most facts (stack, tests, deploy config). Confirm what was detected, then ask only the rest.

**Round 1: scope**
1. `App` | Is this the app to audit? (detected: stack, entry points) | Yes, this repo · A sub-folder (I'll name it) · Several apps in this repo · Another repo / path
2. `Why` | Why audit now? | About to launch · Something is broken or slow · I inherited this code · Regular health check
3. `Runs` | Where does it run today? | Only on my machine · Staging · Production with real users · Not runnable yet
4. `Scale` | How many users? | Just me / testing · Under 100 · Hundreds to thousands · More

**Round 2: priorities**
5. `Worry` | Biggest worry? | Security · Bugs and crashes · Speed / cost · Looks and usability
6. `Areas` | What should be audited? (say all that apply) | Everything (Recommended) · Code and tests · Security · UI · Production readiness
7. `Data` | Sensitive data inside? | Passwords / personal data · Payments · Health or regulated data · Nothing sensitive
8. `Login` | How do users sign in? | Own email + password · Hosted auth (Firebase, Supabase, Clerk, Auth0) · Social login · No login

**Round 3: testing**
9. `Tests` | What tests exist? | A good suite · Some tests · None · Not sure, check
10. `Depth` | How deep should testing go? | Run what exists · Run + fill gaps on core flows · Full: gaps + break-it testing · Match the worry
11. `Run it` | May I start the app locally to test it? | Yes · Yes, but it needs setup (I'll help) · No, read code only
12. `Live` | Any checks against the live app? | None, local only · Read-only: I'll share dashboards / logs · Public pages only (no logins, no writes) · Not live
12b. `Strix` | Add an automated AI penetration test in the security stage? Strix actively attacks your running app the way a real intruder would and surfaces exploitable holes a code read alone misses, so you ship on proof, not hope. Strongly recommended before any launch. | Yes, run Strix (Recommended) · No, checklist only | *Asked every run. Your own app only (local or a target you own); needs Docker and an AI API key; report-only, findings join the ranked list.*

**Round 4: limits and output**
13. `Off limits` | Anything I must not touch? | Production data · Dependencies / versions · Certain files (I'll list) · Nothing
14. `Time` | How thorough? | Quick (about an hour) · Standard (half a day) · Thorough (as long as it takes)
15. `Deliver fixes` | Where should fixes go? | New branch + PR · Current branch · Patch files only · Report only, no fixes
16. `Reader` | Who reads the report? | Me · My team · A client / non-technical person · An investor or buyer (due diligence)

Then ask Visuals and Teamwork mode (see protocol). Teamwork here is the default for every stage; the user can change it at any checkpoint.

## The project brief
After the interview, write `<project>/.claude/project-brief.md`:
- First lines: `Run by: /audit-master` and `Mode: report only` (changed to `Mode: fixing` only in stage 7)
- Every answer above plus what Phase 0 detected and defaults taken
- A **Stages** table: stage · master · status · findings count
- A **Findings** table that every stage appends to: id · severity (Critical / High / Medium / Low) · effort (S / M / L) · area · file:line · finding · suggested fix · stage
Every stage master reads this file first (see protocol Phase 0), asks only what it does not cover, and appends its findings instead of fixing.

## Stages
Run in this order. Skip a stage only when the brief makes it pointless, and say why.

| # | Stage | Master | Runs when |
|---|---|---|---|
| 1 | Understand the app | `/codemap-master` | Always: map, how to run and test it, hot spots |
| 2 | Tests and break-it testing | `/debug-master` | Always; depth from Depth, no fixes yet |
| 3 | Security and code review | `/review-master` | Always: security checklist plus bugs and over-engineering. Strix = yes: also run the `strix` CLI against a local or user-owned instance (needs Docker + an AI API key; confirm the target is theirs); its findings join the Findings table. |
| 4 | UI and accessibility | `/ui-master` (review only) | The app has screens and Areas includes UI or Everything |
| 5 | Production readiness | `/ops-master` | Runs is staging or production, or Why is launch |
| 6 | Ranked fix list | (this master) | Always: merge, de-duplicate, rank by severity then effort |
| 7 | Fix and verify | `/refactor-master` | Deliver fixes is not report only; only the fixes the user ticked |
| 8 | Docs and release | `/docs-master` then `/release-master` | Fixes were applied: README / changelog update, branch + PR |

**Calling a stage master:** invoke it with the Skill tool (or its plugin name, e.g. `masters:debug-master`). If it is not installed, tell the user which master zip or plugin to install, and offer to continue that stage with the closest available skills.

## Checkpoints (after every stage)
Show a short checkpoint, then wait:
1. What the stage found: counts by severity and the top 3 findings, with its key visual
2. What the next stage will do
Options: **Continue** · **Dig deeper here** · **Skip next stage** · **Stop and see the report**.

**Stage 6, the fix pick:** show the ranked list (Critical first, then quick wins), pre-tick Critical and High findings with effort S or M, and let the user change the selection. Group related findings so one fix closes several.

**Stage 7 rules:** one fix at a time, smallest change at the right layer, re-run the test or check that found it, and mark it fixed only with evidence. Never touch production, push, deploy or publish without a fresh yes at that moment.

## Child skills and routing
The children are the other masters, listed in **Stages**. Routing is the "Runs when" column. In report-only mode every stage master appends to the Findings table instead of changing code.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `diagram` | App map from stage 1: modules, data stores, outside services, hot spots in color |
| Live | `board` | Stage board: done, running, next (updated at every checkpoint) |
| Decisions | `table` | Ranked fix list with severity, effort and pre-ticked picks |
| Report | `heatmap` | Findings by area (tests, security, code, UI, ops) by severity |
| Report | `diff-bars` | Before vs after for applied fixes (tests passing, issues open, key timings) |
| Report | `before-after` | UI screens before and after fixes, at desktop and mobile widths |

## Output
1. **Health verdict**: healthy / fix soon / not safe to ship, with a grade (A to F) per area
2. **Findings summary**: counts by severity and area, and the top 5 risks in plain words
3. **Fixed**: finding · change · proof (test or check re-run)
4. **Still open**: ranked, with suggested fixes and effort
5. **How to run and test the app** (from stage 1, verified)
6. **Not verified** and **one next step**
