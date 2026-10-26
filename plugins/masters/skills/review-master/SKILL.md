---
name: review-master
description: "Master command for code review and code quality: reviewing a diff, branch or PR, multi-agent PR review, responding to review feedback, over-engineering audits, whole-project security audits (HTTPS, cookies and sessions, endpoint auth, login hardening, error leaks, dependencies), deferred-shortcut ledgers, and babysitting a PR until it merges. Interviews the user (role, target, focus, depth, risk, standards, what to do with findings), shows a brief, then runs the review skills together. Use only when the user types /review-master or asks for the review master."
---

# Review Master

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

**Round 1: role and target**
1. `Role` | Which side are you on? | Review my work before merge · Review someone else's change · I got feedback, help me respond · Audit the whole repo
2. `Target` | What is under review? *(detect)* | Uncommitted changes · A branch or PR (name or number) · Specific files · The whole repo
3. `Focus` | Main concern? | Correctness bugs · Security audit (full checklist below) · Over-engineering (what to delete) · Everything (incl. tests, error handling, types)
4. `Depth` | How deep? | Quick: top findings only · Standard: every changed line · Exhaustive: multi-agent, multi-pass · Terse: one line per finding

**Round 2: context**
5. `Intent` | Where is the intent of the change written? | PR description / ticket · I'll explain · Infer it from the diff · A spec file
6. `Risk` | Riskiest area? | Security / auth · Data / migrations · Performance / concurrency · Low risk (UI, docs, refactor)
7. `Standards` | Which standards? *(detect)* | The repo's style guide · Language idioms · Google engineering practices · My team's checklist (I'll paste it)
8. `Tests` | Test expectations? | Must have tests · Nice to have · Review the existing tests only · Not applicable

**Round 3: action**
9. `Findings` | What happens with findings? | Report only · Fix after I approve · Auto-fix trivial nits · Post as PR comments (asks before posting)
10. `Feedback` | (If responding) how to treat the feedback? | Verify each point first · Trust and implement all · Push back where doubtful · Not applicable
11. `PR state` | Does the PR need tending? *(detect)* | CI failing · Waiting on review · Unresolved comments · Not a PR
12. `Bar` | Approval bar? | Improves code health (not perfect, just better) · Zero blockers · Security-clean · Informational only

**Round 4: security** (only if Focus = Security audit or Risk = Security / auth)
13. `App type` | What are we securing? *(detect)* | Web app with its own login · API / backend only · Uses a hosted auth provider (Firebase, Supabase, Clerk, Auth0) · Desktop / mobile app with a backend
14. `Live` | Where does it run? *(detect)* | Production with real users · Staging · Local only · Not deployed yet
15. `Sensitive` | What data does it hold? | Passwords / personal data · Payments · Health or other regulated data · Nothing sensitive
16. `Checks` | How far should checks go? | Read the code only · Also run dependency audits · Also probe the running local app (login, rate limits) · All of it

**Always, every run**
`Strix` | Add an automated AI penetration test? Strix actively exercises your running app the way a real attacker would and catches exploitable holes a code read alone misses, turning "looks secure" into proof. Strongly recommended before any launch. | Yes, run Strix (Recommended) · No, checklist only | *Your own app only (local or a target you own); needs Docker and an AI API key; report-only, fixes still wait for your approval.*

Effort comes first and Change level follows the protocol (Phase 1). Skip any question Recon or the user's message already answered; *(detect)* marks the ones most likely to be found automatically. At Quick, ask at most one round of these questions and default the rest.

## Quality bar
**Must do**
- Baseline first: run build, tests, lint and type-check on the target.
- Review in order: design, functionality, complexity, tests, naming, comments, docs. Read every changed line and its context.
- Confirm each finding by reading the code or running a test; label anything you cannot show as "unconfirmed".
- Check that tests would fail if the code broke.
- Security items that need runtime proof (HTTPS, HSTS, cookie flags, headers) are checked with a request against a local or staging instance, or marked "unverified in production".
- The dependency audit needs network; offline means "skipped", never "clean".
- In a staged run, take the Strix answer from the brief instead of asking again.

**Severity**: Blocker = bug, security hole or data loss (must fix) · Major = should fix before merge · Minor = fix soon · Nit = optional, prefixed "Nit:".
**Bar → verdict**: Zero blockers: any Blocker or Major → REQUEST CHANGES · Improves code health: Blocker → REQUEST CHANGES, otherwise APPROVE (WITH NITS) · Security-clean: any security finding Major or worse → BLOCK.

**Gates**: every finding has `file:line` plus evidence · reviewed files listed · dependency audit clean at high severity, or the findings listed.

## Expert playbook
Read `reference/playbook.md` before Phase 2 and before delegating to any child. It holds tool routing, patches to apply on top of each child, method additions, gate additions and the sources to require. On any conflict the playbook beats the child; copy its gate additions into SPEC.md Done-when.

## Child skills and routing
| Child | Use when |
|---|---|
| requesting-code-review | Reviewing your own work before merge. |
| receiving-code-review | Responding to feedback; verify before implementing. |
| code-review-pr | Fast, low-noise GitHub PR comments only; never the correctness pass. Comments only after a yes. |
| review-pr | Exhaustive depth: five report-only specialist agents (code reviewer, comment analyzer, test analyzer, silent-failure hunter, type-design analyzer); the simplifier only when Focus = over-engineering. Agent prompts are in `children/review-pr/agents/`. |
| caveman-review | Terse depth: one line per finding. |
| caveman-evidence-review | Only if the user names Caveman Cloud cost or evidence data. Not code review. |
| ponytail-review | Over-engineering in a diff. |
| ponytail-audit | Over-engineering across the whole repo. |
| ponytail-debt | Listing deferred `shortcut:` / `ponytail:` markers. |
| babysit | A live PR with failing CI, waiting reviews or open comments. |
| differential-review | Security review of a diff, branch or PR: blast radius, git blame, test gaps. |
| audit-context-building | Before a deep security audit of unfamiliar code: map what each function assumes and guarantees. |
| semgrep · codeql · sarif-parsing | Static analysis. semgrep first (fast); codeql for deep data-flow; sarif-parsing to read either tool's output. Needs the tool installed. |
| fp-check | Every security finding before it reaches the table: true or false positive, with evidence. |
| supply-chain-risk-auditor | Dependency audit: advisories, abandoned upstreams, risky publishers. Use before the no-skill fallback below. |
| security-review (built in, not bundled) | Security audit of pending branch changes. For a whole-project audit, walk the **Security checklist** below instead. |
| Dependency audit (no skill) | Checks includes dependency audits: run the project's own tool (`npm audit`, `pnpm audit`, `uvx pip-audit`, `cargo audit`), list vulnerable, unmaintained and unused packages with the exact upgrade or removal command. Never upgrade a major version without a yes. |
| /debug-master → torture-test | Checks includes probing the running app: hand off for adversarial login, rate-limit and input testing. Local only, never production. |
| strix (CLI, not a skill) | Strix = yes. An agentic AI pen-test tool (`strix-agent`, run `strix`). Confirm the target is an app the user owns and point it at a local instance or their own staging, never a third-party site. Needs Docker running and an AI API key in the environment; if either is missing, say exactly what to set up and continue with the checklist. Feed its findings into the same severity table; fixes still wait for approval. |

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
- `differential-review`
- `audit-context-building`
- `fp-check`
- `semgrep`
- `codeql`
- `sarif-parsing`
- `supply-chain-risk-auditor`


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
| differential-review plugin | `differential-review` | `/plugin marketplace add trailofbits/skills` then `/plugin install differential-review@trailofbits` |
| audit-context-building plugin | `audit-context-building` | `/plugin marketplace add trailofbits/skills` then `/plugin install audit-context-building@trailofbits` |
| fp-check plugin | `fp-check` | `/plugin marketplace add trailofbits/skills` then `/plugin install fp-check@trailofbits` |
| static-analysis plugin | `semgrep`, `codeql`, `sarif-parsing` | `/plugin marketplace add trailofbits/skills` then `/plugin install static-analysis@trailofbits` |
| supply-chain-risk-auditor plugin | `supply-chain-risk-auditor` | `/plugin marketplace add trailofbits/skills` then `/plugin install supply-chain-risk-auditor@trailofbits` |
