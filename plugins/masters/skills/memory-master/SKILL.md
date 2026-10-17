---
name: memory-master
description: "Master command for past work and usage: searching what was done before, timelines, weekly digests and standups, token and cost reports, impact receipts for a manager, knowledge bases built from past sessions, memory maintenance and sync. Interviews the user (task, period, project, topic, report type, audience, contents, format, sources), shows a brief, then runs the memory and reporting skills together. Use only when the user types /memory-master or asks for the memory master."
---

# Memory Master

## How this master runs

Follow these phases in order. Do not skip the brief.

### Phase 0: Look before asking
Scan what is already known: the user's message, open files, the repo (stack, configs, README, DESIGN.md, brand files, tests, git state), and memory. Any intake question already answered here is skipped. Tell the user in one line what you detected ("Detected: Next.js + Tailwind, existing site, no brand kit").

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

**Round 1: task**
1. `Task` | What do you need? | Find past work · A report (timeline, digest, usage, receipt) · Ask questions of a knowledge base · Maintenance (sync, align, modes, lessons)
2. `Period` | Time range? | Today · This week · This month · Custom range
3. `Project` | Which projects? | This one · All · A named project · One session
4. `Topic` | Looking for? | A keyword or error · Why we decided X · A feature's history · Everything

**Round 2: reports** (skip if not a report)
5. `Report` | Which report? | Timeline story · Weekly digest / standup · Token and cost usage (HTML) · Impact receipt (what shipped, per project)
6. `Reader` | Who reads it? | Me · My team · My manager · Public
7. `Contents` | Include? | Shipped work · Decisions · Blockers with owners · Numbers
8. `Format` | Delivered as? | Chat · Markdown file · HTML / web page · Recurring on a schedule

**Round 3**
9. `Source` | Where to look? | claude-mem memory · Session transcripts · Screen history (screenpipe) · All of them
10. `Privacy` | Reports can include prompt text and project names. | Keep it on this computer · Strip prompts, keep counts · Fine to share

Then ask Teamwork mode (see protocol).

## Child skills and routing
| Child | Use when |
|---|---|
| mem-search | Find past work. |
| knowledge-agent | Build and query a corpus from past observations. |
| timeline-report | Timeline story. |
| weekly-digests · standup | Weekly digest or standup. |
| agent-cost-report | Agent spend from claude-mem. |
| session-report | Token, cache, subagent and skill usage as an HTML report from transcripts. |
| receipts | Per-project impact receipt (files, lines, commits, PRs, spend share) as HTML + CSV. |
| claude-md-improver | Capturing lessons into CLAUDE.md. |
| caveman-learn | Capturing lessons in caveman format. |
| ccs-align · cloud-sync · mode-creator · how-it-works · screenpipe | Maintenance tasks, as named. |
| diagnosing-superpowers | Skills not triggering or a broken setup. |

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
- `mode-creator`
- `how-it-works`
- `screenpipe`
- `diagnosing-superpowers`


## Safety overrides
These rules apply to these children whether they come bundled or installed:
- `session-report`: remove external font links from the HTML so the report stays offline.


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| claude-mem plugin | `mem-search`, `knowledge-agent`, `timeline-report`, `weekly-digests`, `standup`, `agent-cost-report`, `ccs-align`, `cloud-sync`, `mode-creator`, `how-it-works`, `screenpipe` | `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem@thedotmack` |
| session-report plugin (Anthropic marketplace) | `session-report` | `/plugin install session-report@claude-plugins-official` |
| receipts plugin (Anthropic marketplace) | `receipts` | `/plugin install receipts@claude-plugins-official` |
| claude-md-management plugin (Anthropic marketplace) | `claude-md-improver` | `/plugin install claude-md-management@claude-plugins-official` |
| caveman plugin | `caveman-learn` | `/plugin marketplace add JuliusBrussee/caveman` then `/plugin install caveman@caveman` |
| Superpowers (obra) | `diagnosing-superpowers` | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
