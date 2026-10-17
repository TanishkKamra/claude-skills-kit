---
name: release-master
description: "Master command for git and GitHub housekeeping and releases: committing, pushing and opening PRs, cleaning stale branches, triaging issues, version bumps (SemVer or CalVer), changelogs, tagging and publishing releases. Interviews the user (task, repo, issue scope, allowed GitHub actions, bump, changelog, gates, how far to go), shows a brief, then runs the release skills together. Use only when the user types /release-master or asks for the release master."
---

# Release Master

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

**Round 1: what**
1. `Task` | What do you need? | Commit / push / open a PR · Triage issues · Version bump + changelog · Full release (triage, bump, tag, publish)
2. `Repo` | Which repo? *(detect)* | This one · Another repo (owner/name) · A single issue or PR (number) · All my repos
3. `Allowed` | What may I change on GitHub? | Nothing, report only · Labels · Labels + comments · Labels, comments, close, release (each confirmed)
4. `Branches` | Clean up branches? | Delete local branches whose remote is gone · List them only · Leave them · Not relevant

**Round 2: issues** (skip if no triage)
5. `Issues` | Which issues? | Untriaged · Stale (over 30 days) · Bugs only · Everything
6. `Axes` | Triage by? | Type + priority · Type, severity and priority · The repo's existing labels · Create a label set
7. `Upstream` | Check upstream first? | No · Dependency changelogs / breaking changes · Duplicate issues · Both

**Round 3: release** (skip if no release)
8. `Bump` | Version bump? | Work it out from the changes (SemVer) · Major · Minor · Patch
9. `Scheme` | Versioning scheme? *(detect)* | SemVer · CalVer · 0.x (unstable) · Other
10. `Changelog` | Changelog? | Keep-a-Changelog from merged PRs · From commits · Edit my existing file · None
11. `Gates` | Checks before release? | Tests + build green · Plus docs and breaking-change notes · Plus a security scan · None
12. `How far` | How far should I go? | Prepare only (diff, no commit) · Commit + tag locally · Push + open PR · Push, tag and publish

Effort comes first and Change level follows the protocol (Phase 1). Skip any question Recon or the user's message already answered; *(detect)* marks the ones most likely to be found automatically. At Quick, ask at most one round of these questions and default the rest.

## Quality bar
**Preflight (always, before anything)**: `git status`, branch, remote, last tag, unpushed commits, `gh auth status`, CI state on HEAD, branch protection, the new tag does not exist yet, versions consistent across manifest, lockfile and changelog.

**Must do**
- Plain "commit this": collapse the interview to one confirm.
- Name the public API before choosing the bump: breaking → MAJOR, compatible feature or deprecation → MINOR, compatible fix → PATCH.
- Never move or reuse a released tag; fix forward.
- Curate the changelog for users in Keep a Changelog format with an ISO date; not a pasted commit log.
- Scan the staged diff for secrets (.env, keys, tokens) before every commit and push.
- Branch cleanup: list each branch with its unmerged commit count and get a yes for the whole list before deleting any. Use `git branch -d` only; a branch that needs `-D` needs its own yes naming the command.
- Build and test from a clean checkout of the tag.
- After publishing, verify with `gh release view <tag>` and `git ls-remote --tags`, and report the URL.

**Guards**: breaking change shipped as minor → compare the public API before and after · moved tags → tags are immutable · unreproducible release → clean-checkout build.

**Gates**: tag `v<version>` exists on the remote · changelog entry dated today · clean-checkout build and tests pass · release URL reported.

## Expert playbook
Read `reference/playbook.md` before Phase 2 and before delegating to any child. It holds tool routing, patches to apply on top of each child, method additions, gate additions and the sources to require. On any conflict the playbook beats the child; copy its gate additions into SPEC.md Done-when.

## Child skills and routing
| Child | Use when |
|---|---|
| commit | Writing a commit from staged and unstaged changes. |
| release | Not used: claude-mem-specific. Follow the playbook's generic bump procedure. |
| childhood | After a release is published or deployed: verify what users actually get, watch late CI and reports. |
| commit-push-pr | Commit, push and open a PR in one go (asks first). |
| clean-gone | Removing local branches whose remote branch is gone. |
| oh-my-issues | Issue triage. Read-only mode when Allowed = report only. Triage before release. |
| version-bump | Not used: claude-mem-specific (pushes and publishes). Follow the playbook's generic bump procedure; auto bump: breaking = MAJOR, feature = MINOR, fix = PATCH; 0.x allows breaking changes in MINOR. |
| finishing-a-development-branch | Push, PR or merge at the end. |
| verification-before-completion | Release gates. |
| `gh` CLI (not a skill) | All GitHub work: issues, PRs, releases. Check `gh auth status` first; if not logged in, ask the user to run `gh auth login`. |

Rules: every push, tag, release, comment or close needs a fresh yes at that moment, even if allowed in the interview.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `timeline` | Commits/PRs since the last release, grouped by breaking / feature / fix |
| Decisions | `options` | Version bump options (patch/minor/major) with the reason for each |
| Report | `table` | Triage results: issue, labels, priority, action |
| Report | `diagram` | Branch graph: what was merged, tagged, pushed (or still waiting for your yes) |

## Output
1. **Triage**: N issues · labels applied · duplicates · needs-info · closed
2. **Priority queue**: top 5 with reasons
3. **Version**: old → new (breaking / feature / fix)
4. **Changelog**: Added / Changed / Fixed / Security highlights
5. **Gates**: results
6. **State**: prepared, tagged, pushed or released (+ link)
7. **Waiting for your yes**: anything not yet pushed or published

## Child skills
Not bundled: use the installed versions (see sources below).
- `commit`
- `commit-push-pr`
- `clean-gone`
- `oh-my-issues`
- `version-bump`
- `release`
- `childhood`
- `finishing-a-development-branch`
- `verification-before-completion`


## Safety overrides
These rules apply to these children whether they come bundled or installed:
- `commit-push-pr`: never stage secrets (.env, keys, tokens); create branches with `git checkout -b`; ask before pushing.
- `clean-gone`: list every [gone] branch and worktree first and wait for a yes; delete with `git branch -d` only, never `-D` or `--force`.
- `release`: show the version, changelog and publish targets and wait for a yes before tagging or publishing.
- `childhood`: monitor and report; rollbacks, hotfix releases and announcements each need a fresh yes.


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| commit-commands plugin (Anthropic marketplace) | `commit`, `commit-push-pr`, `clean-gone` | `/plugin install commit-commands@claude-plugins-official` |
| claude-mem plugin | `oh-my-issues`, `version-bump`, `release`, `childhood` | `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem@thedotmack` |
| Superpowers (obra) | `finishing-a-development-branch`, `verification-before-completion` | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
