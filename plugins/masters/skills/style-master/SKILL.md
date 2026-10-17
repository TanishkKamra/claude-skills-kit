---
name: style-master
description: "Master command for how Claude talks and how many tokens it spends: terse reply modes (lite to ultra), compressing CLAUDE.md and memory files, commit message style, forcing complete output, delegating search to compressed subagents, and measuring token savings. Interviews the user, shows a brief, then sets up the right output-style skills. Use only when the user types /style-master or asks for the style master."
---

# Style Master

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

**Round 1: goal**
1. `Goal` | What do you want? | Shorter replies · Shrink memory / instruction files · Set a commit message style · Measure token savings
2. `Level` | How terse? | Lite (short but normal sentences) · Full (caveman default) · Ultra (telegraphic) · Max (fewest words possible)
3. `Scope` | Where should it apply? | This session only · This repo (always on) · All projects · Only for subagents
4. `Keep full` | What must stay in full detail? | Code, commands and errors · Docs and prose too · Nothing · Ask per task

**Round 2: files and commits** (skip what doesn't apply)
5. `Files` | Which files to compress? | CLAUDE.md · Memory files · To-do / notes · I'll list paths
6. `Safety` | How careful with originals? | Backup copy first · Show a diff first · Overwrite · Dry run, report only
7. `Commits` | Commit style? | Conventional (type(scope): message) · Terse intent only · Subject + why body · Keep what I have
8. `Measure` | What to measure? | Token savings before / after · Session cost · Ponytail impact scoreboard · All

**Round 3: behaviour**
9. `Long output` | When output is long? | Always complete, never "rest unchanged" · Shortest wins · Ask when large
10. `Search` | Send code searches to compressed helper agents? | Yes (investigator, builder, reviewer crew) · Exploration only · No, work inline

Then ask Teamwork mode (see protocol). For this master, Solo is usually right.

## Child skills and routing
| Child | Use when |
|---|---|
| caveman | Level = Lite or Full. |
| ultracave | Level = Ultra. |
| megacave | Level = Max. |
| cavecrew | Search = Yes, or Scope = subagents. |
| caveman-explore | Search = exploration only. |
| caveman-compress | Goal = shrink files; pass Files and Safety. |
| caveman-commit | Commit style. |
| caveman-stats | Token savings and cost. |
| ponytail-gain | Ponytail scoreboard. |
| full-output-enforcement | Long output = always complete (code and deliverables only). |
| caveman-optimize · caveman-setup · caveman-help · ponytail-help | Tuning, first-time setup, or "what can I do?" |

Note: always-on modes across sessions need the caveman / ponytail plugins in Claude Code; the skills alone last for the current chat.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `table` | Current setup: active modes, file sizes, commit style |
| Decisions | `options` | Same sample reply at each terseness level, side by side |
| Report | `diff-bars` | Tokens or file size before vs after |
| Report | `table` | What changed and how to undo it |

## Output
```
Mode: <skill> @ <level>, scope <session | repo | global>
Kept in full: <...>
Actions: <skill → result, one line each>
Files changed: <path (backup path)>
Measured: <before → after | cost | gain>
Undo: <"stop caveman" | restore backup | revert commit>
Next: <one step>
```

## Child skills
Not bundled: use the installed versions (see sources below).
- `caveman`
- `ultracave`
- `megacave`
- `cavecrew`
- `caveman-explore`
- `caveman-compress`
- `caveman-commit`
- `caveman-stats`
- `ponytail-gain`
- `full-output-enforcement`
- `caveman-optimize`
- `caveman-setup`
- `caveman-help`
- `ponytail-help`


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| caveman plugin | `caveman`, `ultracave`, `megacave`, `cavecrew`, `caveman-explore`, `caveman-compress`, `caveman-commit`, `caveman-stats`, `caveman-optimize`, `caveman-setup`, `caveman-help` | `/plugin marketplace add JuliusBrussee/caveman` then `/plugin install caveman@caveman` |
| ponytail plugin | `ponytail-gain`, `ponytail-help` | `/plugin marketplace add DietrichGebert/ponytail` then `/plugin install ponytail@ponytail` |
| Taste skills (Leonxlnx) | `full-output-enforcement` | `/plugin marketplace add Leonxlnx/taste-skill` then `/plugin install taste-skill@taste-skill` |
