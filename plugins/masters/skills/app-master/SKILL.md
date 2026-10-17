---
name: app-master
description: "Master command for building a whole app, site or tool from scratch. Runs one complete upfront interview (idea, users, features, data, accounts, look and feel, tech, hosting, scale, launch), writes a project brief, then runs the other master commands stage by stage (research, plan, visuals, UI, build, test, security review, production readiness, docs, release) with a checkpoint after each stage. Use only when the user types /app-master or asks for the app master."
---

# App Master

Builds something new end to end by running the other masters in order. It does not re-implement them: each stage is a master command reading the shared project brief.

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

Then ask Visuals and Teamwork mode (see protocol). Teamwork here is the default for every stage; the user can change it at any checkpoint.

## The project brief
After the interview, write `<project>/.claude/project-brief.md`:
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

**Calling a stage master:** invoke it with the Skill tool (or its plugin name, e.g. `masters:ui-master`). If it is not installed, tell the user which master zip or plugin to install, and offer to continue that stage with the closest available skills.

## Checkpoints (after every stage)
Show a short checkpoint, then wait:
1. What the stage produced (files, screenshots, results), with its key visual
2. What changed in the brief
3. What the next stage will do
Options: **Continue** · **Change something** (re-run this stage with the change) · **Skip next stage** · **Stop here**.
Never deploy, push, publish, pay or send anything without a fresh yes at that moment, even if Go live was chosen.

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
