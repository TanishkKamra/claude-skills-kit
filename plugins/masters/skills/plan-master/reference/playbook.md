# plan-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Routing overrides
- Invoke the top-level installed superpowers skills when present (their relative links resolve there); bundled `children/` copies are the fallback.
- **make-plan** does Phase 0 doc discovery only. **writing-plans** writes the plan, saved to `<run folder>/PLAN.md` (not `docs/superpowers/plans/`).
- **subagent-driven-development / executing-plans**: their "rulings, not stalls" rule is overridden. A ruling that changes scope, a Done-when criterion, a dependency or user-visible behaviour is a STOP: ask the user. Log every other ruling as a `Ruling:` line in PROGRESS.md. SDD never runs implementers in parallel; pass "specify model" as the Agent tool's `model` parameter.
- **executing-plans** for Teamwork = Solo or Quick; it needs a worktree and TDD, so say so in the brief.
- **feature-dev** when a feature needs codebase exploration and architecture options; its Phase 5 approval is the brief's go, so do not ask twice.
- **do** never pushes: commit locally on the run branch; ask for a fresh yes naming the exact command before any push.
- **using-git-worktrees**: print the detected install commands and run none unless the brief listed them. Do not commit `.gitignore` edits; add `.worktrees/` to `.git/info/exclude`. Prefer Claude Code's native `EnterWorktree` when available.
- **dispatching-parallel-agents**: only for tasks whose files do not overlap (write a file-overlap map first); use `isolation: "worktree"`; each return under 300 words with evidence.
- **test-driven-development**: never delete user-written code; stash it in the run folder. On legacy code write characterization tests first.
- **standup** is a multi-branch consolidation chat, not a status report. For status, summarise PROGRESS.md.
- **build / build-and-release** only when the user says "autopilot", and they still pass the four hard gates (they depend on `babysit`, which may be missing).
- **handoff**: write to the run folder and redact secrets.
- **finishing-a-development-branch**: check CI is green and write a PR description (what and why) before offering merge; any `git pull`/`merge` needs a yes naming the command.
- Shell snippets in children are POSIX: run them in Git Bash on Windows.

## Method additions (Deep and Max)
- **Pre-mortem** (Klein): "It is 3 months later and this failed. Why?" List the top 5 causes; each gets a mitigation or a named risk in SPEC.md.
- **ADR** for every hard-to-reverse decision (Nygard): context, decision, consequences, status.
- **Slice into demoable increments** (tracer bullet): each task ends in something runnable and checkable; small batches (Google eng-practices small CLs, DORA).
- **Appetite** (Shape Up): state the time box; cut scope, not quality, to fit it.
- **Context reset**: after two failed corrections on the same task, start a fresh subagent with a rewritten prompt instead of piling on.

## Gate additions
- Every plan task names its files, its check command and its Done-when line.
- Pre-mortem and ADRs present at Deep and Max.

## Sources to require
Anthropic Claude Code best practices (explore, plan, implement, commit) · Google eng-practices · Nygard ADRs, adr.github.io · Malte Ubl "Design Docs at Google" · Basecamp *Shape Up* · *The Pragmatic Programmer* (tracer bullets) · Klein pre-mortem (HBR) · obra/superpowers · Humble and Farley *Continuous Delivery* · *Accelerate* / DORA.
