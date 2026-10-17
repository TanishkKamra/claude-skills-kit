# Baseline, evidence and independent verification

## Baseline (before the first change)
1. Record `git status` and the branch. How to handle uncommitted changes was settled in Phase 1; apply that answer, do not ask again. Never mix the user's changes into yours.
2. Create a checkpoint so everything can be rolled back:
   - git, Deep/Max: a new branch `master-run/<short-task>`.
   - git, Quick, clean tree: record `git rev-parse HEAD`; no branch.
   - no git: copy every file the plan will change into `<run folder>/backup/`; the verifier compares against those copies.
3. Run what exists: build, type-check, lint, the test suite (or the relevant part if the full suite is very slow; say which). Record pass/fail counts and failing test names in `EVIDENCE.md` under **Baseline**.
4. Non-code work: baseline = the current state of the artifact (screenshot of the current UI, current doc page count, current measurements).
Pre-existing failures are not yours to fix unless the spec says so, but you must not add new ones.

## EVIDENCE.md
```
# Evidence
## Baseline
build: ok | tests: 142 passed, 3 failed (test_a, test_b, test_c) | lint: 12 warnings
## Steps
| Step | Check run | Key output | Result |
## Done-when
| # | Proof run | Key output line | PASS/FAIL/NOT VERIFIED |
```
Paste real output lines, not paraphrases. A row with no output is NOT VERIFIED.

## What counts as a real check
- It actually ran and you saw its output. A command that failed to start, timed out or was skipped is not a pass.
- It exercises behaviour, not just syntax. For UI and apps, drive the running thing (browser, CLI) and confirm the user-visible outcome.
- If a dependency is missing, install it with the project's own package manager (rule 6 applies: ask first). If no real check is possible, say which check was skipped and why.
- Full regression: rerun the same suite as the baseline and compare. Any test that passed at baseline and fails now is your regression to fix.

## The critic (Phase 4, Deep and Max) and the verifier (Phase 7)
Spawn a **fresh** general-purpose subagent. Give it only: `SPEC.md`, the plan or the diff (`git diff <checkpoint>`, or a file-by-file comparison with `backup/` when there is no git), `EVIDENCE.md`, and the paths of produced artifacts. Do not give it your reasoning or conclusions.

Critic prompt (before go):
> You are reviewing a plan before work starts. Read SPEC.md and the plan. Report only: criteria that are not checkable, missing criteria a user would expect, plan steps that cannot satisfy a criterion, risks not guarded, wrong assumptions you can disprove by reading the repo. Each finding: one line + evidence. Max 10 findings. If the plan is sound, say "no blocking issues".

Verifier prompt (after build):
> Try to refute that this work meets SPEC.md. Re-run every proof in the Done-when table yourself and compare with EVIDENCE.md. Read the diff for correctness bugs, regressions, scope creep and weakened tests. Report only gaps that affect correctness or a stated criterion, each with evidence (command + output, or file:line). Do not report style preferences. End with VERDICT: PASS or VERDICT: FAIL (n issues).

## Fix rounds
- Verifier FAIL: fix the reported issues, rerun the affected proofs, update `EVIDENCE.md`, and run a fresh verifier. **At most 3 rounds.** After round 3, stop and report what still fails, with evidence and what was tried.
- Never resolve a finding by weakening a test or a criterion.
- Disagree with a finding? Show the evidence in the report instead of silently ignoring it.

## Max effort extras
- **Red team**: after the verifier passes, one more fresh subagent with a different brief: "find the input, sequence or environment that breaks this" (edge cases, empty/huge inputs, concurrency, permissions, offline, slow network, unusual locale). It ends with `VERDICT: NO CONFIRMED BREAK` or a list of confirmed breaks with repro. Fix confirmed breaks (at most 2 rounds), then run a fresh verifier once more.
- **Second verifier**: for design or writing, a second verifier judges only the taste criteria against the references.

## Quick self-check (replaces the verifier at Quick)
Rerun every Done-when proof, re-read the whole diff once against `SPEC.md` looking for regressions, scope creep and weakened tests, and record `VERDICT: self-check PASS/FAIL` in `RUN.md`.
