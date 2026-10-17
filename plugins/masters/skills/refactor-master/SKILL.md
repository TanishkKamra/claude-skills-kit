---
name: refactor-master
description: "Master command for changing code safely: bug fixes at the narrowest layer, refactors with no behavior change, library, API or schema migrations, minimal new builds, and simplifying over-built code. Interviews the user (change type, blast radius, what must not change, risk, tests, rollback, git), shows a brief, then runs the change skills together. Use only when the user types /refactor-master or asks for the refactor master."
---

# Refactor Master

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

**Round 1: what**
1. `Type` | What kind of change? | Bug fix · Refactor (same behavior) · Migration (library, API, data, schema) · Minimal new build
2. `Goal` | Desired outcome? | Fix a specific symptom · Cleaner structure · Move to a new version or tech · Smallest thing that works
3. `Blast` | Blast radius? | One function or file · One module · Cross-cutting · Unknown
4. `Known` | What do you know already? | Repro + stack trace · Vague symptom · Exact location · Nothing yet

**Round 2: limits**
5. `Keep` | What must NOT change? | Public API · Data formats / schema · Output and UI · Everything outside the target
6. `Deps` | Dependencies? | Use the repo as is · Pin versions · No new dependencies · Upgrades allowed
7. `Risk` | Risk tolerance? | Low: reversible steps + rollback plan · Medium · High: big-bang is fine
8. `Tests` | Testing? | Regression test first · Existing suite must stay green · Add characterization tests if coverage is thin · Manual check only

**Round 3: delivery**
9. `Research` | Research first? | No · Target's changelog and migration guide · Known issues for this bug · Both
10. `Git` | Git handling? | Branch + PR · Worktree · Current branch · No commits
11. `Rollback` | Rollback plan? | One revertable commit · Feature flag · Staged rollout · Not needed
12. `Done` | Definition of done? | Repro fixed, tests green · Plus a minimal-diff check · Plus docs / changelog · Plus a follow-up list

Then ask Teamwork mode (see protocol).

## Child skills and routing
| Child | Use when |
|---|---|
| investigate-first | Known = vague or nothing, or Blast = unknown. Runs before anything else. |
| surgical-patch | Bug fixes, or "everything outside the target" must stay. |
| safe-refactor | Refactors; characterization tests first when coverage is thin. |
| migration | Library, API, data or schema moves. |
| lean-build | Minimal new builds. |
| ponytail | Always on as the "smallest working diff" check. |
| code-simplifier | Tidying recently changed code without changing behavior. Agent prompt in `children/code-simplifier/`. |

Rules: unclear root cause hands off to /debug-master first. Risk = Low requires a written rollback step. Finish with a verification pass.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `diagram` | Blast radius: the target, what depends on it, what must not change |
| Decisions | `options` | Approach options (e.g. flag vs big-bang) with risk and effort |
| Report | `diff-bars` | Lines added/removed per file |
| Report | `diagram` | Structure before vs after (modules and calls), changed parts in color |
| Report | `table` | Behavior checks and tests: before/after results |

## Output
1. **Type / goal**
2. **Root cause or rationale**
3. **Changes**: file · one line each
4. **Behavior preserved**: what was checked
5. **Tests**: added and run, with results
6. **Rollback**: how
7. **Risks / not done**, **git state**, **one next step**

## Child skills
Not bundled: use the installed versions (see sources below).
- `investigate-first`
- `surgical-patch`
- `safe-refactor`
- `migration`
- `lean-build`
- `ponytail`
- `code-simplifier`
- `verification-before-completion`


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| caveman plugin | `investigate-first`, `surgical-patch`, `safe-refactor`, `migration`, `lean-build` | `/plugin marketplace add JuliusBrussee/caveman` then `/plugin install caveman@caveman` |
| ponytail plugin | `ponytail` | `/plugin marketplace add DietrichGebert/ponytail` then `/plugin install ponytail@ponytail` |
| pr-review-toolkit plugin (Anthropic marketplace) | `code-simplifier` | `/plugin install pr-review-toolkit@claude-plugins-official` |
| Superpowers (obra) | `verification-before-completion` | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
