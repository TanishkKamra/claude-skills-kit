# refactor-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Routing overrides
- Unclear root cause: **systematic-debugging** first (not investigate-first, which is a 5-line stub), then surgical-patch.
- **ponytail** runs at `full`, on the diff only; it never changes the report format or pushes back on the user's request.
- **lean-build** for new slices; ponytail is the size check afterwards.
- **migration** follows this master's contract-phase rule over its own "do not contract" line: finish expand, migrate and contract, with the contract step behind a fresh yes if it drops data.
- **code-simplifier** follows only CLAUDE.md and the surrounding file's conventions. Ignore its built-in style rules (ES modules, `function` over arrows, "avoid try/catch", React Props) unless the repo already uses them. Scope = files in `git diff <checkpoint>`. Ask before editing.

## Patches when using children
- **surgical-patch**: write a failing regression test first (if impossible, say why). Prove red to green by reverting the fix and watching the test fail. Grep every caller before editing. Put a one-line root cause in the commit.
- **safe-refactor**: (1) characterization tests at a seam where coverage is thin (Feathers); (2) one named Fowler refactoring per commit; (3) never mix structural and behaviour changes in one commit (Beck, *Tidy First?*); (4) the same suite before and after each step; (5) prefer IDE, LSP or codemod tools over hand edits.
- **investigate-first** (only if chosen): reproduce first, then bisect (`git bisect run`), then "five whys".

## Method additions
- **Before refactoring**: measure coverage on touched files; on Deep/Max run a mutation tool if one exists (Stryker, mutmut, PIT, cargo-mutants) and report survivors.
- **Hyrum's Law check**: list observable behaviours of public APIs (output order, error text, timing, defaults) and keep them unless the user agreed to change them.
- **Large or risky changes**: parallel change (expand/migrate/contract), branch by abstraction or strangler fig; shadow-read compare old and new paths (GitHub Scientist style) before switching.
- **Data backfills**: dry run, count before and after, spot-check 20 rows, idempotent re-run.
- **Rollback**: when Rollback = Feature flag or Staged rollout, or the change is risky in production, add a kill switch; otherwise one revertable commit is enough.
- Large mechanical migrations: Claude Code `/batch` (worktree subagents) when available.

## Gate additions
- Same test suite green before and after; structural and behaviour commits separate.
- No public-behaviour change unless listed in SPEC.md.

## Sources to require
Fowler *Refactoring* 2nd ed and refactoring.com · Feathers *Working Effectively with Legacy Code* · Fowler bliki: ParallelChange, BranchByAbstraction, StranglerFigApplication · Stripe "Online migrations at scale" · GitHub Engineering "Scientist" · *Software Engineering at Google* ch. 22 (large-scale changes) · Beck *Tidy First?* · Hyrum's Law (hyrumslaw.com) · OpenRewrite / codemod docs.
