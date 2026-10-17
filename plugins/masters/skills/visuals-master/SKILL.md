---
name: visuals-master
description: "Master command for brand kits, generated web or mobile mockup images, posters and one-page visuals, and turning an image or screenshot into working code. Interviews the user (task, purpose, style, colors, references, assets, fidelity), shows a brief, then runs the right visual skills together. Use only when the user types /visuals-master or asks for the visuals master."
---

# Visuals Master

## How this master runs

Follow these phases in order. Do not skip the brief.

### Phase 0: Look before asking
Scan what is already known: the user's message, open files, the repo (stack, configs, README, DESIGN.md, brand files, tests, git state), and memory. Any intake question already answered here is skipped.
If `<project>/.claude/project-brief.md` exists, read it first and treat its answers as given. If it starts with `Run by: /app-master`, this master is one stage of a bigger build: ask only questions the brief does not cover, use its Visuals and Teamwork answers, present your Phase 4 brief as that stage's checkpoint (one approval, not two), and update the brief's Stages table when done. Tell the user in one line what you detected ("Detected: Next.js + Tailwind, existing site, no brand kit").

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
1. `Task` | What do you need? | Brand kit (logo, palette, type, rules) · Web mockup images · Mobile app mockup images · Image or screenshot to code
2. `Purpose` | What is it for? | Pitch / approval · Build target (will be coded next) · Marketing, social, poster · Just exploring options
3. `Business` | What is the product or company? | Existing business (I'll describe it) · New idea (help shape it) · Personal / portfolio · Read it from the repo
4. `Audience` | Who is it for? | Consumers · Business / B2B · Developers · A niche community (I'll describe)

**Round 2: look**
5. `Style` | Visual style? | Clean modern SaaS · Editorial / minimal · Bold / brutalist · Premium / dark / cinematic
6. `Tone` | Brand personality? | Trustworthy, professional · Friendly, playful · Bold, edgy · Calm, minimal
7. `Color` | Color direction? | I have exact colors · Light neutral + one accent · Dark + vivid accent · Propose 2 to 3 palettes
8. `Variants` | How many options? | 1 best guess · 3 distinct directions · One at a time with feedback · Full set of key screens

**Round 3: inputs**
9. `References` | Inspiration? | I have links / screenshots · Research online and build a mood board · Competitors (I'll name them) · None
10. `Assets` | Existing assets? | Full kit · Logo only · Nothing, create from scratch · A screenshot / mockup to convert (I'll attach it)
11. `Platform` | Target? | Desktop web · Mobile app (iOS / Android) · Both · Print / poster / social sizes
12. `Fidelity` | How far should it go? | Images only · Images + tokens / spec · Images + coded build · Code only, from my image

Then ask Teamwork mode (see protocol).

## Child skills and routing
| Child | Use when |
|---|---|
| brandkit | Task = Brand kit, or Assets = Nothing and a build needs a brand. Runs first. |
| imagegen-frontend-web | Web mockups. |
| imagegen-frontend-mobile | Mobile mockups. |
| canvas-design | Posters, one-page visuals, art, PNG / PDF pieces, print or social sizes. |
| image-to-code | Converting a provided or generated image into code. Always last in a chain. |

Rules: brandkit before image generation when no brand exists. Purpose = Build target: after approval, offer to hand off to /ui-master with the chosen direction.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `swatches` | Palette, type and tone chosen for the brand |
| Decisions | `options` | Variant gallery: every generated option side by side with notes |
| Report | `before-after` | For image-to-code: source image vs coded result, with deviations listed |
| Report | `table` | Deliverables list: file, size, recommended variant |

## Output
1. **Brief recap**: task, purpose, audience, platform, style, palette
2. **Decisions**: direction, tokens (colors, type, radius, spacing), references (links), defaults taken
3. **Skills run** (and judge table if used)
4. **Deliverables**: image paths named by screen and variant, brand kit files, code files; which variant is recommended
5. **Verification**: text legible, brand consistent across pieces, correct sizes; for image-to-code a side-by-side vs the source with deviations listed
6. **Not verified** and **one next step**

## Child skills
Not bundled: use the installed versions (see sources below).
- `brandkit`
- `imagegen-frontend-web`
- `imagegen-frontend-mobile`
- `canvas-design`
- `image-to-code`


## Safety overrides
These rules apply to these children whether they come bundled or installed:
- `canvas-design`: use only the bundled fonts; ask before downloading any other font.


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| Taste skills (Leonxlnx) | `brandkit`, `imagegen-frontend-web`, `imagegen-frontend-mobile`, `image-to-code` | `/plugin marketplace add Leonxlnx/taste-skill` then `/plugin install taste-skill@taste-skill` |
| Anthropic example skills | `canvas-design` | `/plugin marketplace add anthropics/skills` then `/plugin install example-skills@anthropic-agent-skills` |
