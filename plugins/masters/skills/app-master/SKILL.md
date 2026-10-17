---
name: app-master
description: "Master command for building a whole app, site or tool from scratch. Runs one complete upfront interview (idea, users, features, data, accounts, look and feel, tech, hosting, scale, launch), writes a project brief, then runs the other master commands stage by stage (research, plan, visuals, UI, build, test, security review, production readiness, docs, release) with a checkpoint after each stage. Use only when the user types /app-master or asks for the app master."
---

# App Master

Builds something new end to end by running the other masters in order. It does not re-implement them: each stage is a master command reading the shared project brief.

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
One combined interview covering every stage. Ask it in rounds; skip what the user's message already says.

**Round 1: the idea**
1. `Build` | What are we building? | Web app · Mobile app · Website / landing page · Desktop app or tool
2. `Problem` | What problem does it solve? (one sentence, type it) | Saves people time · Makes or saves money · Connects people · Fun / creative / personal
3. `Users` | Who is it for? | Consumers · Businesses / teams · Developers · Just me / my group
4. `Stage` | How far should this go? | Clickable prototype · MVP real people can use · Production-ready launch · Learning project

**Round 2: features and data**
5. `Features` | Must-have features? (say all that apply) | Accounts and profiles · Payments / subscriptions · Real-time (chat, live updates) · Content, uploads or search
6. `Data` | What does it store? | User content (posts, files) · Business records (orders, bookings) · Personal / sensitive data · Little or nothing
7. `Connects` | Outside services? | Payments (Stripe etc.) · Email / SMS / notifications · AI / LLM features · Maps, calendars or other APIs
8. `Admin` | Does someone manage it? | Admin dashboard needed · Simple settings page · Edit data directly · No admin

**Round 3: accounts and scale**
9. `Login` | How do people sign in? | Email + password · Google / Apple / social · Magic link or OTP · No login
10. `Roles` | Different kinds of users? | Just one kind · User + admin · Several roles / teams · Not sure yet
11. `Scale` | How many users in the first year? | Under 100 · Hundreds · Thousands · More
12. `Rules` | Anything legal or sensitive? | Payments / tax · Health / kids / regulated data · GDPR / privacy rules · Nothing special

**Round 4: look and feel**
13. `Theme` | A site or app to replicate, or a brand to match? | Yes, I'll name or link it · Match my existing brand · Show me directions · Design freely
14. `Look` | Visual direction? | Minimal editorial · Bold / brutalist · Premium / high-end · Friendly and playful
15. `Brand` | Brand assets? | Full kit · Logo only · Create a brand kit for me · Not needed
16. `References` | Research before designing? | I'll share references · Research competitors and inspiration online · Both · No

**Round 5: tech and hosting**
17. `Stack` | Tech preference? | Pick the best fit for me · Next.js / React · Python backend · Something specific (I'll say)
18. `Platform` | Where must it run? | Desktop browsers · Mobile browsers too · Native iOS / Android · Desktop app
19. `Hosting` | Where will it live? | Free tier / cheapest · Managed (Vercel, Firebase, Render) · My own server · Decide later
20. `Budget` | Monthly running cost limit? | Free only · Under $20 · Under $100 · Whatever it takes

**Round 6: quality and launch**
21. `Deadline` | When do you need it? | Today / this weekend · This week · This month · No rush
22. `Tests` | How solid must it be? | Quick checks · Tests for core flows · Full test suite + break-it testing · Match the stage
23. `Go live` | Deploy it at the end? | Yes, deploy and check production readiness · Get it deploy-ready, I'll deploy · Local only
24. `Repo` | Code and releases? | New GitHub repo (private) · Use an existing repo · Local git only · No git
25. `Strix` | Add an automated AI penetration test in the security stage? It actively exercises the running app the way a real attacker would and proves holes a code read misses. Own app only; needs Docker and an AI API key. | Yes (Recommended when there is a login or personal data) · No, checklist only

Effort comes first and Change level follows the protocol (Phase 1). Skip any question Recon or the user's message already answered; *(detect)* marks the ones most likely to be found automatically. At Quick, ask at most one round of these questions and default the rest. Teamwork here is the default for every stage; the user can change it at any checkpoint.

## The project brief
After the interview, write `<project>/.master-runs/project-brief.md`:
- First line: `Run by: /app-master`
- Every answer above, grouped by round, plus defaults taken
- A **Stages** table: stage · master · status (pending / running / done / skipped) · notes
Update it after every stage. Every stage master reads this file first (see protocol Phase 0) and asks only what it does not cover.

## Stages
Run in this order. Skip a stage only when the brief makes it pointless, and say why.

| # | Stage | Master | Runs when |
|---|---|---|---|
| 1 | Research | `/research-master` | References = research online, or the idea needs market or tech facts |
| 2 | Plan and architecture | `/plan-master` (planning only) | Always |
| 3 | Brand and mockups | `/visuals-master` | Brand = create a kit, or Theme = show me directions |
| 4 | UI design | `/ui-master` | Anything with screens |
| 5 | Build | `/plan-master` (execution) | Always |
| 6 | Test and fix | `/debug-master` | Always; depth from Tests |
| 7 | Security and code review | `/review-master` (security audit) | Always; full checklist when Login or sensitive Data |
| 8 | Production readiness | `/ops-master` | Go live = deploy or deploy-ready |
| 9 | Docs | `/docs-master` | Always: README with run, test and deploy steps |
| 10 | Git and release | `/release-master` | Repo is not "No git" |
| 11 | Deploy | (this master) | Go live = deploy: fresh yes, deploy, smoke-test the live URL, record the rollback command in the brief |

**Calling a stage master:** invoke it with the Skill tool (or its plugin name, e.g. `masters:ui-master`). If it is not installed, tell the user which master zip or plugin to install, and offer to continue that stage with the closest available skills.

## Checkpoints (after every stage)
Show a short checkpoint, then wait:
1. What the stage produced (files, screenshots, results), with its key visual
2. What changed in the brief
3. What the next stage will do
Options: **Continue** · **Change something** (re-run this stage with the change) · **Skip next stage** · **Stop here**.
Never deploy, push, publish, pay or send anything without a fresh yes at that moment, even if Go live was chosen.

## Quality bar
**Must do**
- The brief lists MVP features (at most 7), each with an acceptance test, plus non-goals, a decisions log (stack + versions + why) and artifact paths.
- The research stage may revise the Stack and Hosting answers; log every change in the brief.
- Login, Data and Rules answers become requirements in the plan stage, not only checks in the review stage.
- Walking skeleton first: a thin end-to-end slice that runs before any feature work.
- Tests go in the same change as each slice; follow the test pyramid (many unit, fewer integration, very few end-to-end).
- Deploy is its own step: fresh yes, a smoke test of the live URL, and a recorded rollback command.
- README run, test and deploy commands are verified from a clean checkout.

**Stage presets (write into the brief before each stage; anything else that makes no sense for a new app is n/a)**:
- plan: Goal = New user-facing feature, Clarity = Written spec (this brief), Size = Multi-day, Research = Skip (stage 1 did it), Git / Finish / Done = from Repo, Go live, Tests
- ui: Start = From scratch; Project, Goal, Audience, Theme, Look, Mood, References, Brand, Content from Rounds 1 and 4
- debug: Goal = Verify finished work, Symptom / Repro / Expected / Suspect / Severity = n/a, Approach = Regression test after the fix, Proof = Tests + build green, QA depth = from Tests
- review: Role = Audit the whole repo, Focus = Security audit, Findings = Fix after I approve, Strix = from the brief
- ops: Why = Launching soon, Scale / Hosting / Budget = from Rounds 3 and 5, Areas = Everything, Checks = Also run local load or failure tests, Fixes = Fix what I approve
- docs: Format = Document (README), Audience = Developers, Length = Lightning, File = Markdown
- release: Task = Full release, Allowed = labels, comments, releases (each confirmed), Bump = Work it out, How far = from Go live and Repo

**Gates**: clean-checkout setup, build and test using only README commands · every MVP feature's acceptance test passes · review and ops stage gates recorded as passed.

## Child skills and routing
The children are the other masters, listed in **Stages**. Routing is the "Runs when" column. When two stages could merge (for example a tiny landing page needs no separate visuals stage), propose merging at the checkpoint instead of silently skipping.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `diagram` | App map: screens, features and data, with the outside services they use |
| Brief | `flow` | Stage pipeline: which stages run, which are skipped and why |
| Decisions | `options` | Each stage's key choice side by side (architecture, design direction) |
| Live | `board` | Stage board: done, running, next (updated at every checkpoint) |
| Report | `table` | Final scorecard: tests, security checklist, readiness verdict, release state |
| Report | `before-after` | Mockup vs built screens at desktop and mobile widths |

## Output
1. **What was built**: one paragraph, plus the app map
2. **How to run it**: exact commands, URLs, environment variables needed
3. **Stage results**: stage · master · result · link to its section of the visual report
4. **Quality**: tests passed, security checklist summary, readiness verdict
5. **Release**: repo, version, deploy URL (if deployed)
6. **Not verified**, **open items**, and **one next step**
