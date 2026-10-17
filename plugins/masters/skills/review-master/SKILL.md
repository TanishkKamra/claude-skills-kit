---
name: review-master
description: "Master command for code review and code quality: reviewing a diff, branch or PR, multi-agent PR review, responding to review feedback, over-engineering audits, whole-project security audits (HTTPS, cookies and sessions, endpoint auth, login hardening, error leaks, dependencies), deferred-shortcut ledgers, and babysitting a PR until it merges. Interviews the user (role, target, focus, depth, risk, standards, what to do with findings), shows a brief, then runs the review skills together. Use only when the user types /review-master or asks for the review master."
---

# Review Master

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

**Round 1: role and target**
1. `Role` | Which side are you on? | Review my work before merge · Review someone else's change · I got feedback, help me respond · Audit the whole repo
2. `Target` | What is under review? | Uncommitted changes · A branch or PR (name or number) · Specific files · The whole repo
3. `Focus` | Main concern? | Correctness bugs · Security audit (full checklist below) · Over-engineering (what to delete) · Everything (incl. tests, error handling, types)
4. `Depth` | How deep? | Quick: top findings only · Standard: every changed line · Exhaustive: multi-agent, multi-pass · Terse: one line per finding

**Round 2: context**
5. `Intent` | Where is the intent of the change written? | PR description / ticket · I'll explain · Infer it from the diff · A spec file
6. `Risk` | Riskiest area? | Security / auth · Data / migrations · Performance / concurrency · Low risk (UI, docs, refactor)
7. `Standards` | Which standards? | The repo's style guide · Language idioms · Google engineering practices · My team's checklist (I'll paste it)
8. `Tests` | Test expectations? | Must have tests · Nice to have · Review the existing tests only · Not applicable

**Round 3: action**
9. `Findings` | What happens with findings? | Report only · Fix after I approve · Auto-fix trivial nits · Post as PR comments (asks before posting)
10. `Feedback` | (If responding) how to treat the feedback? | Verify each point first · Trust and implement all · Push back where doubtful · Not applicable
11. `PR state` | Does the PR need tending? | CI failing · Waiting on review · Unresolved comments · Not a PR
12. `Bar` | Approval bar? | Improves code health (not perfect, just better) · Zero blockers · Security-clean · Informational only

**Round 4: security** (only if Focus = Security audit or Risk = Security / auth)
13. `App type` | What are we securing? | Web app with its own login · API / backend only · Uses a hosted auth provider (Firebase, Supabase, Clerk, Auth0) · Desktop / mobile app with a backend
14. `Live` | Where does it run? | Production with real users · Staging · Local only · Not deployed yet
15. `Sensitive` | What data does it hold? | Passwords / personal data · Payments · Health or other regulated data · Nothing sensitive
16. `Checks` | How far should checks go? | Read the code only · Also run dependency audits · Also probe the running local app (login, rate limits) · All of it

Then ask Teamwork mode (see protocol).

## Child skills and routing
| Child | Use when |
|---|---|
| requesting-code-review | Reviewing your own work before merge. |
| receiving-code-review | Responding to feedback; verify before implementing. |
| code-review-pr | PR review with several agents and confidence scoring that filters false positives. |
| review-pr | Exhaustive depth: six specialist agents (code reviewer, simplifier, comment analyzer, test analyzer, silent-failure hunter, type-design analyzer). Agent prompts are in `children/review-pr/agents/`. |
| caveman-review | Terse depth: one line per finding. |
| caveman-evidence-review | Reviewing Caveman Cloud cost and evidence data. |
| ponytail-review | Over-engineering in a diff. |
| ponytail-audit | Over-engineering across the whole repo. |
| ponytail-debt | Listing deferred `ponytail:` shortcuts. |
| babysit | A live PR with failing CI, waiting reviews or open comments. |
| security-review (built in, not bundled) | Security audit of pending branch changes. For a whole-project audit, walk the **Security checklist** below instead. |
| Dependency audit (no skill) | Checks includes dependency audits: run the project's own tool (`npm audit`, `pnpm audit`, `uvx pip-audit`, `cargo audit`), list vulnerable, unmaintained and unused packages with the exact upgrade or removal command. Never upgrade a major version without a yes. |
| /debug-master → torture-test | Checks includes probing the running app: hand off for adversarial login, rate-limit and input testing. Local only, never production. |

Rules: merge all child findings into one table and remove duplicates by file:line. Posting comments, pushing or merging always needs a fresh yes.

## Security checklist
Walk every item that applies; mark each **pass / fail / not applicable** with file:line evidence. Fixes only after the user approves.
1. **HTTPS everywhere**: HTTPS enforced on every route, HSTS header set, no mixed content, nothing sensitive over plain HTTP.
2. **Cookies and sessions**: session cookies are HttpOnly, Secure, SameSite=Strict (or Lax with a reason); idle timeout (default 30 min); session fully destroyed on logout; session ID regenerated right after login.
3. **Every endpoint checks auth on the server**: authenticated and authorised before returning data; no endpoint works without a valid session or token; responses carry only the fields the frontend uses.
4. **No client-side-only checks**: admin/role checks, password-strength rules and validation are enforced on the server, not just in the UI.
5. **No guessable IDs (IDOR)**: records looked up by sequential or user-supplied IDs are checked against the caller; prefer unguessable IDs; nothing sensitive in URLs or query strings.
6. **Login hardening**: rate limiting and lockout on /login, password reset and OTP endpoints; 2FA / OTP offered where accounts matter; password strength checked server-side; reset tokens single-use and short-lived; no "user not found" vs "wrong password" difference.
7. **Error handling**: no raw errors, stack traces or internal paths reach users in production; clean human-readable messages; details go to logs only.
8. **Dependencies**: no known-vulnerable, unmaintained or unused packages (see Dependency audit above).
9. **Secrets**: no keys or tokens in the repo, client bundle or logs; secrets come from env / a secrets manager.
10. **Injection and request forgery**: parameterised queries (SQL injection), output encoding (XSS), CSRF protection on state-changing requests, CORS limited to known origins, no server-side fetch of user-supplied URLs without an allow-list (SSRF).
11. **Tokens and access**: JWTs short-lived with refresh-token rotation and revocation; signing keys rotatable; service accounts, cloud IAM roles and database users have least privilege; OAuth scopes minimal.
12. **Data protection at the edge and at rest**: sensitive data encrypted at rest; TLS on internal hops too; public apps sit behind a WAF / DDoS protection or the host's built-in equivalent.
With a hosted auth provider, check its settings and security rules (e.g. Firestore rules) instead of re-implementing login.

## Visual aids
Shown as described in the protocol (inline + the visual report page). Component = the block to copy from `visuals/report-template.html`.

| Moment | Component | Shows |
|---|---|---|
| Brief | `diff-bars` | What is under review: files and size of change |
| Report | `heatmap` | Findings per file by severity (blocker / major / minor / nit) |
| Report | `table` | Findings: severity, file:line, problem, fix |
| Report | `diff-bars` | Delete list: lines that could go |
| Report | `table` | Security scorecard: each checklist item pass / fail / n.a. with file:line |

## Output
1. **Header**: target, intent, verdict (APPROVE / APPROVE WITH NITS / REQUEST CHANGES / BLOCK), bar used
2. **Findings**: # · severity (Blocker, Major, Minor, Nit) · category · file:line · problem · fix · source skill
3. **Delete list** (over-engineering), **deferred debt**
4. **Good things** (one or two)
5. **Not reviewed** and **one next step**

## Child skills
Not bundled: use the installed versions (see sources below).
- `requesting-code-review`
- `receiving-code-review`
- `code-review-pr`
- `review-pr`
- `caveman-review`
- `caveman-evidence-review`
- `ponytail-review`
- `ponytail-audit`
- `ponytail-debt`
- `babysit`


## Safety overrides
These rules apply to these children whether they come bundled or installed:
- `code-review-pr`: show the full PR comment and wait for a yes before posting it.


## Where child skills come from
If a child is neither installed nor bundled, show the user its install line and ask whether to continue without it.

| Source | Children | Install |
|---|---|---|
| Superpowers (obra) | `requesting-code-review`, `receiving-code-review` | `/plugin marketplace add obra/superpowers-marketplace` then `/plugin install superpowers@superpowers-marketplace` |
| code-review plugin (Anthropic marketplace) | `code-review-pr` | `/plugin install code-review@claude-plugins-official` |
| pr-review-toolkit plugin (Anthropic marketplace) | `review-pr` | `/plugin install pr-review-toolkit@claude-plugins-official` |
| caveman plugin | `caveman-review`, `caveman-evidence-review` | `/plugin marketplace add JuliusBrussee/caveman` then `/plugin install caveman@caveman` |
| ponytail plugin | `ponytail-review`, `ponytail-audit`, `ponytail-debt` | `/plugin marketplace add DietrichGebert/ponytail` then `/plugin install ponytail@ponytail` |
| claude-mem plugin | `babysit` | `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem@thedotmack` |
