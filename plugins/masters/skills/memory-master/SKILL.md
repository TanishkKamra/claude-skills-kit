---
name: memory-master
description: "Master command for past work and usage: searching what was done before, timelines, weekly digests and standups, token and cost reports, impact receipts for a manager, knowledge bases built from past sessions, memory maintenance and sync. Interviews the user (task, period, project, topic, report type, audience, contents, format, sources), shows a brief, then runs the memory and reporting skills together. Use only when the user types /memory-master or asks for the memory master."
---

# Memory Master

## Start here: four hard gates (every run, every effort level, even for "small" tasks)
1. **First action:** create the run folder and `PROGRESS.md` (Quick: `RUN.md`) containing the Run sequence checklist below. If the run folder cannot be written (permission denied, read-only), keep the same sections in your replies instead and carry on; never stop the task over this.
2. **No project file is edited** until `SPEC.md` (Quick: the Spec section of `RUN.md`) exists with Done-when criteria.
3. **No final message** until `EVIDENCE.md` (Quick: `RUN.md`) holds a `VERDICT:` line: Deep/Max from a verifier subagent you started with the Agent tool; Quick from your self-check.
4. **The final message starts with** `Run folder: <path>` and the checklist `0 ✓ 1 ✓ … 8 ✓`, every phase ticked or marked `skipped: <reason>`. Never tick a phase you did not do.
A task feeling small is not a reason to skip a gate; Quick effort is the light path, and it still passes all four.

## Ground rules (these hold for the whole run, every phase)
1. **Done means proven.** Before building, write testable acceptance criteria ("Done when") into `SPEC.md`, each naming the command, file, screenshot or page that proves it and the expected result. Never report a criterion as done without its evidence in `EVIDENCE.md`. A check that did not run, or only checked syntax, does not count.
2. **Read before you claim.** Never state anything about code, files, data or a page you have not opened in this run. Cite `file:line`, a URL or a command output for every factual claim.
3. **Current docs over memory.** Before using any library, framework, CLI, API or file format you did not just read in this project, check its exact version (lockfile, `--version`) and read current docs (context7, vendor docs, changelog). If docs are unreachable, mark it "from memory, unverified".
4. **Ask only what changes the outcome.** Skip every question that Recon or the user's message already answered. Ask only what changes the plan and cannot be found. Every assumption goes into `SPEC.md` under Assumptions and into the report.
5. **No go, no build.** Before the user approves the brief you may only read, run read-only commands (run the failing command, tests, logs, `git log`/`blame`) and write files inside the run folder. The brief lists every risky command the plan needs (for example `npm install x`); the go covers exactly those. After go, keep working through every step without stopping, except for a blocker or a risky action not covered by the go.
6. **Fresh yes for risky actions.** Ask, naming the exact command, before: push, publish, merge, deploy, send, post, pay; any destructive git command (reset, clean, force, branch -D); deleting or overwriting files the user wrote; DB migrations or data writes outside local dev; installing, upgrading or removing dependencies; anything against a non-local host; anything with metered cost (paid APIs, image generation, extra agents beyond the chosen effort).
7. **Never weaken the bar to pass it.** Do not edit, skip or special-case tests or criteria to turn them green. If a test or criterion looks wrong, stop and tell the user why.
8. **Stay in scope.** Change only what the criteria require. Put other improvements in the report as suggestions.
9. **Independent check before you report (Deep and Max).** A fresh subagent that did not do the work verifies against `SPEC.md` (Phase 7, `reference/verify.md`). Up to 3 fix rounds. **The run is not finished until `EVIDENCE.md` contains the verifier's `VERDICT:` line, and the report quotes it.** Quick replaces this with the self-check in Phase 7.
10. **State lives in files.** `SPEC.md` and `PROGRESS.md` start with the Run settings line (`effort= teamwork= visuals= live board= change level=`). Keep `PROGRESS.md` current after every step. If you lose track (long run, compacted context), re-read `PROGRESS.md`, `SPEC.md` and `EVIDENCE.md` before doing anything else.

**Run folder:** `<project>/.master-runs/<YYYY-MM-DD>-<master>-<short-task>/` holds `CONTEXT.md`, `SPEC.md`, `PROGRESS.md`, `EVIDENCE.md` and `report.html` (Quick: one `RUN.md` with SPEC, PROGRESS and EVIDENCE sections). If `.git` exists, add `.master-runs/` to `.git/info/exclude` so it is never committed. With no project folder (claude.ai), keep the same sections in the chat or an Artifact.

**Staged runs:** if `<project>/.master-runs/project-brief.md` exists, read `reference/orchestration.md` before anything else and follow its stage rules.

## Run sequence
Copy this checklist into `PROGRESS.md` and tick it as you go. Each phase opens its reference file first.

| # | Phase | Read first | Gate: do not move on until |
|---|---|---|---|
| 0 | Recon | `reference/recon.md` | `CONTEXT.md` written; a "Detected:" block shown |
| 1 | Interview | this file | Effort chosen first; every outcome-changing question answered or defaulted |
| 2 | Research | `reference/playbook.md` (if present), `reference/recon.md` §4 and `reference/sources.md` | every library/API the plan uses has a doc source and version in `CONTEXT.md` |
| 3 | Spec | `reference/spec.md` | `SPEC.md` has goal, non-goals, Done-when with proofs, assumptions, risks |
| 4 | Brief | this file | the user said go (one approval) |
| 5 | Baseline | `reference/verify.md` | baseline recorded in `EVIDENCE.md`; a checkpoint to roll back to exists |
| 6 | Build | master sections below | every step's check passed and logged; `PROGRESS.md` current |
| 7 | Verify | `reference/verify.md` | every criterion has evidence or is NOT VERIFIED; Quick: self-check done; Deep/Max: verifier `VERDICT:` recorded |
| 8 | Report | Output section below | evidence per criterion, assumptions, Not verified, one next step |

Failures (tool missing, app will not start, tests absent, network down): follow `reference/failures.md`. Token budget (usage limits, model per subagent, pausing safely): follow `reference/budget.md` from Phase 1 onwards.

## Effort levels
| | Quick | Deep | Max |
|---|---|---|---|
| Recon | `recon.md` items 1, 2, 4, 7, 8 only | full checklist; explorers for large repos | full checklist + explorers |
| Interview | Effort + at most ONE round of the master's questions; no Change level when the user said "fix only X" | Effort, then the needed rounds | same as Deep |
| Research (Phase 2) | only if the fix relies on behaviour not already seen working here | always for libraries/APIs used | always, plus 2 alternative sources |
| Spec | 2 to 4 criteria, one `Risk:` line | 4 to 8 criteria + pre-mortem | as many as needed + red-team criterion |
| Brief | text only | with the Brief visual; critic subagent first | same as Deep |
| Baseline | tests covering the touched area; checkpoint = `git rev-parse HEAD` if the tree is clean | full suite; own branch | full suite; own branch |
| Verify | self-check: rerun every proof, re-read the diff once against SPEC | independent verifier, up to 3 fix rounds | verifier + red team (2 fix rounds) + second verifier for taste criteria or after a red-team fix |
| Approaches | one | one | two competing approaches, each by its own read-only subagent, judged against SPEC before building (even if Teamwork is not Panel) |
| Visuals | Text only | as chosen | as chosen |
| Cost | 1× | about 2 to 3× | about 5 to 10× (state the estimate in the brief) |

## Phase 0: Recon
Read `reference/recon.md` and work through its checklist (Quick: the reduced list above) before asking anything. Then show one "Detected:" block: what this is, stack and versions, what exists, what is missing, repo state (git or not, dirty or clean), what could not be checked.

## Phase 1: Interview
- **Round 1 always starts with Effort** (Quick · Deep · Max, with the costs from the table). Mark one "(Recommended)" by task size from Recon and say why in one line: **Quick** when the task is small and clear (one or two files, a known cause or an exact request, low risk); **Deep** for normal work; **Max** when it is risky (security, data, money, production, public release), cross-cutting or multi-day. The user can pick any level. Ask it together with the master's most important question. At Deep and Max also ask **Teamwork** (Solo: one best-fit skill · Relay: skills take turns, each handing its result to the next · Panel + judge: 2 to 3 skills propose, a judge scores them against the spec, one builds the winner; default Deep → Relay, Max → Panel) and **Visuals** (Key moments (Recommended) · Full · Text only; then offer a live progress board, warning it costs about 10 to 25% more tokens, default No). Quick: Solo and Text only, do not ask.
- **Change level** (not at Quick when the user asked for a specific fix; never inside a staged run): when the target already exists (repo, UI, document, brand, plan), ask "How much should change?" with *Keep as is: fix only what I asked* · *Light touch* · *Significant rework* · *Full rebuild*. Then for every existing decision the plan would alter (stack, structure, layout, naming, design direction, data model, wording), ask one question showing **current → proposed** with *Keep current* / *Change* / *Change differently (I'll say)*, 4 per round. The user stays in control of every change to what exists.
- **Scope and repo state:** confirm what is in and out ("Detected X; I'll cover A and B, not C. Right?"). If the git tree is dirty, ask now: stash, commit, or new branch. If there is no git, say the checkpoint will be a backup copy in the run folder.
- **The master's own Intake:** ask only what is still open. Mark detected answers "(detected)" in the brief so the user can overrule them.
- Use AskUserQuestion where available: up to 4 questions per round, 2 to 4 options, recommended option first with "(Recommended)". Elsewhere ask in chat as a numbered list with lettered options and wait. Links and paths in answers are references to read. "You decide" gets a default, logged under Assumptions.

## Phase 2: Research
If `reference/playbook.md` exists, read it first: its routing, child patches and required sources apply to the whole run. Follow `reference/recon.md` section 4 for every library, API and format the plan uses (see Effort levels for how much). Use only sources that pass `reference/sources.md`: Tier 1 first, Tier 2 only cross-checked, and note the tier of each source. When the user asks to replicate, duplicate or rebuild something, follow the rules at the end of `sources.md`.

## Phase 3: Spec
Write `SPEC.md` per `reference/spec.md`, then go straight to the brief.

## Phase 4: Brief, then wait
Show one screen:
1. Goal, scope, non-goals, change level
2. **Done when**: the criteria from `SPEC.md`, each with its proof
3. Detected facts and defaults taken (overridable)
4. Research findings and doc sources, if any
5. Child skills and their order (teamwork and effort already chosen); a token-cost range per `reference/budget.md`
6. Plan: numbered steps, files or artifacts touched, top risks, and every risky command the plan needs
7. The Brief visual from **Visual aids** (not at Quick or Text only)
At Deep and Max, first send `SPEC.md` and the plan to a fresh critic subagent (prompt in `reference/verify.md`) and fix what it finds. Then ask for **go** or changes. Changing a Done-when criterion after go needs the user's yes.

## Phase 5: Baseline
Follow `reference/verify.md` Baseline. Questions about repo state were settled in Phase 1; do not ask again.

## Phase 6: Build
- **Using a child skill:** if it is installed (in your skill or command list, possibly with a plugin prefix), invoke it with the Skill tool. Otherwise read the bundled copy at `children/<name>/CHILD.md` and follow it. If it is neither, show its install line from **Where child skills come from** and ask whether to continue without it. Masters that have a **Safety overrides** section apply it to those children however they were installed.
- **Bundled agent prompts:** for `children/<name>/agents/*.md`, start a general-purpose subagent with that file's body plus the task.
- **Every delegation** states the goal, the exact files or sources in scope, what to return (under 300 words, with `file:line` or URL evidence) and what not to touch.
- **Solo / Relay:** one step at a time in the brief's order. After each step run its check and append the result to `EVIDENCE.md`. Commit only on the run's own branch (created in Phase 5); with no run branch, leave changes in the working tree.
- **Panel + judge** (and Max's two approaches): proposers are read-only, under 400 words each. The judge scores each against the Done-when criteria (criteria fit 40%, risk 25%, effort 20%, fit to the user's stated taste 15%) and shows the table. A gap under 10 points, or a taste call, goes to the user. One builder executes the winner.
- **Retries:** one retry per failing step, then change approach (write what you learned in `PROGRESS.md` first). If the second approach also fails, report it as a blocker with the evidence.
- A new decision that is the user's to judge: stop and ask it.

## Phase 7: Verify
Rerun every Done-when proof yourself and record the output. Then: Quick → re-read your diff once against `SPEC.md` (self-check). Deep → spawn the independent verifier per `reference/verify.md` and loop up to 3 fix rounds. Max → verifier, then red team, then a second verifier where the table says so. Record each `VERDICT:` line in `EVIDENCE.md`.

## Phase 8: Report
Use the **Output** template below, then always add:
- **Evidence**: each Done-when criterion → PASS / FAIL / NOT VERIFIED, with the command or artifact and its key output line
- **Verifier**: the `VERDICT:` line (Deep/Max) or "self-check" (Quick)
- **Assumptions** made, and how to reverse each
- **Not verified**: anything unchecked and why
- **One next step**

## Visuals
Unless Text only, show the visuals listed under **Visual aids** following `reference/visuals.md`: inline in the chat and on the run's `report.html` (from `visuals/report-template.html`). Real data only; label anything illustrative "sketch".

## Intake

**Round 1: task**
1. `Task` | What do you need? | Find past work · A report (timeline, digest, usage, receipt) · Ask questions of a knowledge base · Maintenance (sync, align, modes, lessons)
2. `Period` | Time range? | Today · This week · This month · Custom range
3. `Project` | Which projects? *(detect)* | This one · All · A named project · One session
4. `Topic` | Looking for? | A keyword or error · Why we decided X · A feature's history · Everything

**Round 2: reports** (skip if not a report)
5. `Report` | Which report? | Timeline story · Weekly digest / standup · Token and cost usage (HTML) · Impact receipt (what shipped, per project)
6. `Reader` | Who reads it? | Me · My team · My manager · Public
7. `Contents` | Include? | Shipped work · Decisions · Blockers with owners · Numbers
8. `Format` | Delivered as? | Chat · Markdown file · HTML / web page · Recurring on a schedule

**Round 3**
9. `Source` | Where to look? | claude-mem memory · Session transcripts · Screen history (screenpipe) · All of them
10. `Privacy` | Reports can include prompt text and project names. | Keep it on this computer · Strip prompts, keep counts · Fine to share

Effort comes first and Change level follows the protocol (Phase 1). Skip any question Recon or the user's message already answered; *(detect)* marks the ones most likely to be found automatically. At Quick, ask at most one round of these questions and default the rest.

## Quality bar
**Must do**
- Privacy first: default "keep on this computer"; list project names before writing anything shareable.
- Search narrow (query, date range, project), then widen once if needed.
- Every recalled fact carries a source pointer (session, observation id, file).
- Verify remembered files, functions and decisions against the current repo before presenting them as true; otherwise mark them "from memory".
- If claude-mem is missing, search session transcripts and git log, and say that coverage is reduced.
- Usage reports state the counting method and time zone, dedupe by session id, and reconcile totals with the raw logs.
- Zero results: say none found, widen once, then stop. Never fill gaps.

**Guards**: confident recall of something never done → source pointer required · stale memory treated as current → date check · double counting → dedupe.

**Gates**: every digest claim has a source pointer that opens · report totals reconcile with the raw sum.

## Expert playbook
Read `reference/playbook.md` before Phase 2 and before delegating to any child. It holds tool routing, patches to apply on top of each child, method additions, gate additions and the sources to require. On any conflict the playbook beats the child; copy its gate additions into SPEC.md Done-when.

## Child skills and routing
| Child | Use when |
|---|---|
| mem-search | Find past work. |
| knowledge-agent | Build and query a corpus from past observations. |
| timeline-report | Timeline story. |
| weekly-digests | Weekly digest or standup. (The standup child is a git branch reconciler, not a report.) |
| agent-cost-report | Agent spend from claude-mem. |
| session-report | Token, cache, subagent and skill usage as an HTML report from transcripts. |
| receipts | Per-project impact receipt (files, lines, commits, PRs, spend share) as HTML + CSV. |
| claude-md-improver | Capturing lessons into CLAUDE.md. |
| caveman-learn | Capturing lessons in caveman format. |
| how-it-works | Explaining how claude-mem works. |
| ccs-align · cloud-sync · mode-creator · screenpipe | Only on explicit request. cloud-sync uploads memory to a paid cloud and screenpipe reads screen history: warn first. |
| backup-setup | Local claude-mem database snapshots: set up, status, restore. Restore overwrites the live database: confirm first. |
| diagnosing-superpowers | Only when superpowers skills are not triggering. |
| owner-goals | The user's own priorities and open asks, with dated quotes, from prompts they typed. |
| request-progress | Progress per request, linked to owner goals (run owner-goals first). |

Rules: most children need the claude-mem plugin in Claude Code; session-report and receipts only need Node. Never publish a report without the user's yes, and list the project names it contains first.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `flow` | What will be searched: sources and period |
| Report | `timeline` | Activity timeline for the period |
| Report | `diff-bars` | Tokens, cost or output per project |
| Report | `table` | Matches: date, session, one line, reference |

## Output
```
Report: <topic / period>
Scope: <project> | <date range> | sources
Shipped · Decisions (decision → reason) · Blockers (item | owner | unblock)
Numbers: <values or [data needed]>
Matches (search): date | session | one line | ref
Files: <report paths>
Gaps · Next
```

## Child skills
Not bundled: use the installed versions (see sources below).
- `mem-search`
- `knowledge-agent`
- `timeline-report`
- `weekly-digests`
- `standup`
- `agent-cost-report`
- `session-report`
- `receipts`
- `claude-md-improver`
- `caveman-learn`
- `ccs-align`
- `cloud-sync`
- `backup-setup`
- `mode-creator`
- `how-it-works`
- `screenpipe`
- `diagnosing-superpowers`
- `owner-goals`
- `request-progress`


## Safety overrides
These rules apply to these children whether they come bundled or installed:
- `session-report`: remove external font links from the HTML so the report stays offline.


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| claude-mem plugin | `mem-search`, `knowledge-agent`, `timeline-report`, `weekly-digests`, `standup`, `agent-cost-report`, `ccs-align`, `cloud-sync`, `backup-setup`, `mode-creator`, `how-it-works`, `screenpipe`, `owner-goals`, `request-progress` | `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem@thedotmack` |
| session-report plugin (Anthropic marketplace) | `session-report` | `/plugin install session-report@claude-plugins-official` |
| receipts plugin (Anthropic marketplace) | `receipts` | `/plugin install receipts@claude-plugins-official` |
| claude-md-management plugin (Anthropic marketplace) | `claude-md-improver` | `/plugin install claude-md-management@claude-plugins-official` |
| caveman plugin | `caveman-learn` | `/plugin marketplace add JuliusBrussee/caveman` then `/plugin install caveman@caveman` |
| Superpowers (obra) | `diagnosing-superpowers` | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
