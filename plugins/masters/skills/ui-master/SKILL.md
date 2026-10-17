---
name: ui-master
description: "Master command for designing, redesigning, polishing or reviewing any user interface: websites, landing pages, web apps, dashboards, components, desktop and mobile screens. Interviews the user (purpose, audience, stack, theme to replicate, references or online research, look and feel), shows a brief, then runs the right design skills together (solo, relay or panel + judge). Use only when the user types /ui-master or asks for the UI master."
---

# UI Master

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
1. `Project` | What are we designing? | Landing / marketing page · Multi-page site, portfolio or editorial · Web app or dashboard · A component or screen set
2. `Start` | Is there existing UI? | From scratch · Redesign (looks dated or generic) · Polish only (works, needs a visual/UX pass) · Review only (audit, no restyle)
3. `Goal` | What is the main outcome? | Convert (signups, sales, leads) · Inform / impress (credibility, story) · Productivity (finish tasks fast) · Launch / demo piece
4. `Audience` | Who uses it? | Consumers · Business buyers (B2B) · Developers / technical users · Internal team / admins

**Round 2: stack and look**
5. `Stack` | Tech stack? | HTML/CSS/JS, no build · React / Next.js (+ Tailwind) · Vue / Svelte / Astro · Native desktop or mobile
6. `Theme` | Is there a specific theme or site to replicate? | Yes, I'll name or link it · Match my existing brand/site · Pick from directions you propose · No, design freely
7. `Look` | Which visual direction? | Minimal editorial (warm monochrome, big type, flat bento) · Brutalist / industrial (raw grids, mono type, terminal feel) · High-end agency (depth, glass, motion, premium) · Not sure: show me 3 directions to pick from
8. `Mood` | Overall feeling? | Calm, trustworthy, professional · Bold, loud, experimental · Playful, friendly, colorful · Technical, dense, data-forward

**Round 3: references, brand, content**
9. `References` | Inspiration sources? | I have links / screenshots · Research online (5 to 8 curated references from Awwwards, Godly, Land-book, Mobbin) and show me · Use competitor sites (I'll name them) · None, decide for me
10. `Brand` | Brand assets? | Full kit (logo, colors, fonts) · Partial (logo or colors) · None: design palette and type too · Extract tokens from the existing site
11. `Content` | Copy and imagery? | Real content ready · Placeholder copy is fine · Write the copy for me · Need images too (generate or source)
12. `Motion` | How much animation? | None / subtle · Moderate (scroll reveals, hovers) · Rich (choreographed, scroll-driven, 3D) · Whatever the direction needs

**Round 4: limits and done**
13. `Must-haves` | Hard constraints? (say all that apply) | Accessibility (WCAG AA, keyboard, reduced motion) · Performance (small bundle, Lighthouse target) · Mobile-first · Quick pass, fewer iterations
14. `Deliver` | What should I hand back? | Design direction only (tokens, style tiles) · Build the pages / screens · Build + screenshot-and-fix loop · Audit report, then fix what I approve
15. `Done` | How do we judge it? | Looks distinctive, not AI-generic · Close match to a reference · Hits a metric (conversion, Lighthouse) · You approve screenshots

Then ask Teamwork mode (see protocol).

## Child skills and routing
| Child | Use when |
|---|---|
| redesign-existing-projects | Start = Redesign. Run first to audit what exists. |
| ui-polish | Start = Polish only, or Deliver = audit then fix. Also its reference-gathering step when References = Research online. |
| web-design-guidelines | Start = Review only, or Accessibility picked. Always the last check. |
| design-taste-frontend | Landing, marketing, portfolio or editorial sites built from scratch. Not dashboards. |
| frontend-design | Distinctive production UI in any stack; good second voice on a panel. |
| impeccable | Web apps, dashboards, product UI, forms, onboarding, native screens; capturing tokens into DESIGN.md. |
| minimalist-ui | Look = Minimal editorial. |
| industrial-brutalist-ui | Look = Brutalist / industrial, or dense data dashboards with that feel. |
| high-end-visual-design | Look = High-end agency, or Motion = Rich. |
| design-is | Look = Not sure: propose 3 directions before building. |
| stitch-design-taste | The user mentions Google Stitch or wants Stitch-ready output. |
| gpt-taste | The user asks for that specific style. |

Rules: pick exactly one primary aesthetic skill. Good panel for Panel + judge: design-taste-frontend or impeccable, frontend-design, plus the aesthetic skill that matches Look. Images needed: hand that part to /visuals-master.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `diagram` | Sitemap or screen map with the pages/sections that will change highlighted |
| Brief | `swatches` | Proposed palette and type pairing for the chosen direction |
| Decisions | `options` | 3 direction tiles side by side (palette, type, a sample section), judge scores if Panel + judge |
| Report | `before-after` | Screenshots before vs after at desktop (1440px) and mobile (390px) |
| Report | `table` | Checks: contrast, keyboard, console errors, Lighthouse (pass/fail) |

## Output
1. **Brief recap** (5 lines): project, goal, audience, stack, scope
2. **Design decisions**: direction and why, palette, type, spacing, motion, references used (links), defaults taken
3. **Skills run**: in order, one line each on what it contributed (and the judge table if Panel + judge)
4. **Deliverables**: files created or changed, DESIGN.md / tokens, screenshots at desktop and mobile widths
5. **Verification**: page loaded, console clean, contrast and keyboard checked, responsive widths, Lighthouse if asked
6. **Not verified** and **one next step**

## Child skills
Not bundled: use the installed versions (see sources below).
- `redesign-existing-projects`
- `ui-polish`
- `web-design-guidelines`
- `design-taste-frontend`
- `frontend-design`
- `impeccable`
- `minimalist-ui`
- `industrial-brutalist-ui`
- `high-end-visual-design`
- `design-is`
- `stitch-design-taste`
- `gpt-taste`


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| Taste skills (Leonxlnx) | `redesign-existing-projects`, `design-taste-frontend`, `minimalist-ui`, `industrial-brutalist-ui`, `high-end-visual-design`, `stitch-design-taste`, `gpt-taste` | `/plugin marketplace add Leonxlnx/taste-skill` then `/plugin install taste-skill@taste-skill` |
| Vercel agent skills | `web-design-guidelines` | copy `skills/web-design-guidelines` from github.com/vercel-labs/agent-skills into `~/.claude/skills/` |
| frontend-design plugin (Anthropic marketplace) | `frontend-design` | `/plugin install frontend-design@claude-plugins-official` |
| impeccable plugin | `impeccable` | `/plugin marketplace add pbakaus/impeccable` then `/plugin install impeccable@impeccable` |
| claude-mem plugin | `design-is` | `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem@thedotmack` |
