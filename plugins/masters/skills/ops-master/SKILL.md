---
name: ops-master
description: "Master command for production readiness: reliability (timeouts, retries, idempotency, health checks, backups), performance (caching, indexes, N+1 queries, connection pooling, latency), observability (logging, metrics, tracing, alerts, SLOs), concurrency (races, locking) and deployment (CI/CD, containers, rollouts, rollbacks, feature flags, cost). Interviews the user about scale, hosting and uptime needs, checks only what fits the app's size, shows a brief, then audits and fixes what the user approves. Use only when the user types /ops-master or asks for the ops master."
---

# Ops Master

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

**Round 1: the app**
1. `App` | What is going live? | Web app + backend · API / backend service · Static site / serverless functions · Desktop or mobile app with a backend
2. `Scale` | How big is it now? | Personal / prototype (a few users) · Small product (hundreds of users) · Growing (thousands, real traffic) · At scale (many servers or regions)
3. `Hosting` | Where does it run? | One server / VPS · Managed platform (Vercel, Firebase, Render, Fly) · Containers / Kubernetes · Not deployed yet
4. `Goal` | Why now? | Launching soon · It broke or got slow · Expecting more traffic · General health check

**Round 2: needs**
5. `Uptime` | How bad is downtime? | Fine, hobby project · Annoying, users notice · Costly, it's a business · Critical, must not go down
6. `Data` | What can never be lost? | User accounts / content · Payments / orders · Nothing critical · Not sure, help me decide
7. `Areas` | What should I check? (say all that apply) | Reliability · Performance · Observability (logs, metrics, alerts) · Deployment and rollback
8. `Known` | Anything already hurting? | Slow pages or queries · Crashes / timeouts · Deploys are scary or manual · Nothing yet

**Round 3: how**
9. `Checks` | How far should checks go? | Read code and config only · Also run local load or failure tests · Also check the live dashboards / logs I give access to · All of it
10. `Fixes` | What happens with findings? | Report only · Fix what I approve · Fix quick wins, ask on the rest
11. `Budget` | Hosting cost matters? | Keep cost flat · Small increases OK · Spend what it takes
12. `Limits` | Anything off limits? | Don't touch production · Don't add new services · No new dependencies · No limits

Then ask Visuals and Teamwork mode (see protocol).

## Child skills and routing
| Child | Use when |
|---|---|
| investigate-first | Always first: map entry points, data stores, external calls and deploy config before judging. |
| systematic-debugging | Known = slow or crashing: find the root cause before tuning anything. |
| torture-test | Checks includes local load or failure tests (timeouts, retries, crashes). Local only, report only. |
| webapp-testing | Measuring page and API timings on the local app. |
| surgical-patch | Fixes = approved fixes, one narrow change at a time. |
| verification-before-completion | Last gate: prove every fix with a before/after measurement or test. |
| /review-master (handoff) | Security items: run its security audit instead of duplicating it here. |
| /release-master (handoff) | Versioning, tagging and publishing after deploy changes. |

Rules: check only the checklist items whose tier fits Scale (see below) and mark the rest "not needed yet" with the trigger that would make them matter. Never change production, live infrastructure or billing without a fresh yes. Prefer the platform's built-in feature over new services.

## Readiness checklist
Tiers: **[all]** every app · **[growing]** from Scale = Growing · **[scale]** only at scale. Mark each pass / fail / not needed yet, with file:line or config evidence.
1. **Timeouts on every outside call** [all]: HTTP clients, DB queries and queues have explicit timeouts; nothing waits forever.
2. **Retries done safely** [all]: retries only on safe or idempotent operations, with exponential backoff and jitter; writes carry idempotency keys; no retry storms.
3. **Health checks** [all]: a cheap health endpoint; liveness vs readiness split on containers / Kubernetes [growing].
4. **Backups and restore** [all]: automated backups of the data that can never be lost, and one restore actually tested; failover and multi-region plans [scale].
5. **Database basics** [all]: indexes on columns used in WHERE / JOIN / ORDER BY; no N+1 queries; connection pooling with limits; read replicas, partitioning, sharding [scale].
6. **Caching** [growing]: cache hot reads with a clear invalidation rule; CDN / edge caching for static assets [all].
7. **Concurrency** [all]: no race conditions on shared writes (counters, balances, stock); optimistic or pessimistic locking where two users can edit the same record; distributed locks [scale].
8. **Resource health** [growing]: no memory leaks under repeat load; backpressure on queues; cold-start and serverless limits known.
9. **Async work** [growing]: slow jobs moved to a queue or cron; failed jobs go to a dead-letter queue and are visible; webhooks verified and retried.
10. **Logging** [all]: structured logs with request IDs; no secrets or personal data in logs; errors logged once with context.
11. **Metrics and alerts** [growing]: request rate, error rate, latency (p50 / p99) tracked; alerts on what users feel, not on every blip; SLOs and error budgets [scale]; distributed tracing across services [scale].
12. **Deploys** [all]: CI runs tests before deploy; builds are reproducible (lockfile, cached builds); one-command rollback that has been tried.
13. **Safer releases** [growing]: feature flags for risky changes; blue-green, canary or rolling deploys; DB migrations that are backward compatible.
14. **Config and infrastructure** [growing]: config and secrets from environment, not code; infrastructure as code (Terraform, Helm) [scale]; autoscaling limits set [scale].
15. **Incidents** [growing]: someone gets alerted; a short runbook for top failure modes; blameless post-mortem template; on-call [scale].
16. **Cost** [all]: no idle or oversized resources; spend alerts set; know which feature costs the most.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `diagram` | System map: app, data stores, outside services, deploy path, risky hops in color |
| Decisions | `options` | Fix options per finding (built-in feature vs new service) with cost and effort |
| Report | `heatmap` | Readiness by area (reliability, performance, observability, deploy) by severity |
| Report | `table` | Readiness scorecard: each checklist item pass / fail / not needed yet with evidence |
| Report | `diff-bars` | Before vs after measurements (latency, query count, error rate) for applied fixes |

## Output
1. **Readiness verdict**: ready / ready with risks / not ready, for the stated scale
2. **Scorecard**: item · tier · status · evidence · fix
3. **Top 3 risks**: what breaks first and how users would notice
4. **Fixes applied** (if approved): change · before → after measurement
5. **Not needed yet**: items skipped and the trigger that makes them matter
6. **Not verified** and **one next step**

## Child skills
Not bundled: use the installed versions (see sources below).
- `investigate-first`
- `systematic-debugging`
- `torture-test`
- `webapp-testing`
- `surgical-patch`
- `verification-before-completion`


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| caveman plugin | `investigate-first`, `surgical-patch` | `/plugin marketplace add JuliusBrussee/caveman` then `/plugin install caveman@caveman` |
| Superpowers (obra) | `systematic-debugging`, `verification-before-completion` | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
| Anthropic example skills | `webapp-testing` | `/plugin marketplace add anthropics/skills` then `/plugin install example-skills@anthropic-agent-skills` |
