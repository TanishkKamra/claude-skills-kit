# debug-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Routing overrides
- **systematic-debugging** always first, with the method additions below pasted into the delegation.
- **test-driven-development**: the Intake Approach overrides its Iron Law. Strict TDD only if chosen; otherwise red-green by revert (test passes with the fix, revert the fix, test fails, restore). Never delete user-written files; stash them in the run folder.
- **playwright-cli** is the primary browser child (exploration and proof); `@playwright/test` files for durable regression tests. Never `attach` to the user's real browser profile or use `--persistent` on one; `state-save` files go to the run folder; prove with snapshots or `getByRole`, not screenshots alone; `playwright-cli close-all` at the end.
- **webapp-testing** only for Python-only projects and never as final proof. Replace `networkidle` and fixed timeouts with web-first assertions (`expect(locator).to_be_visible()`); every script asserts and exits 1 on failure; redirect server output to a file, not PIPE; on Windows kill the process tree (`taskkill /T /F /PID`); artifacts go to the run folder.
- **torture-test** only for "Break it", S1, QA depth = Heavy, or Proof = full adversarial pass. Ask first with an agent-count estimate (max 4 agents). On non-Cadence projects drop the DSP, DAC and VBScript rounds. Findings go to EVIDENCE.md as `# | sev | file:line | repro | CONFIRMED/SUSPECTED`.
- **verification-before-completion** wins over verify-and-stop. Cross-check each proof against the SPEC criterion text; a green command that does not exercise the criterion is NOT VERIFIED. verify-and-stop may reuse a result only if HEAD plus working-tree hash are unchanged and the output is in this run's EVIDENCE.md.

## Method additions (paste into the systematic-debugging delegation)
1. **Hypothesis ledger** in PROGRESS.md: `H# | claim | prediction if true | experiment | result`. One row per experiment; never edit a row, add a new one.
2. **Regression**: `git bisect run <repro>` for "it used to work"; diff lockfiles for dependency regressions.
3. **Minimise** (delta debugging): halve the input or steps until removing anything makes it pass.
4. **Check the plug** (Agans): stale build or cache, wrong env, config, version, clock or locale.
5. **Flaky or race**: fix seeds and clocks, run the repro 20+ times, report the pass rate before and after; use sanitizers (TSan, ASan) or stress runs.
6. **Performance**: profile first (py-spy, Chrome DevTools, dotnet-trace, perf); never guess.
7. **Stuck after 2 hypotheses**: give a fresh read-only subagent only the repro and the evidence.
8. A fix counts only when the original symptom is re-run and the regression test fails without the fix.

## Test quality
- Test at the lowest pyramid level that can fail for this bug; characterization tests on legacy code.
- Parsers, serialisers, money and invariants: one property-based test (Hypothesis, fast-check).
- Deep/Max: run a mutation tool on touched files if one exists (Stryker, mutmut, PIT, cargo-mutants) and report survivors.
- Record an incident-style note: symptom, timeline, root cause, fix, regression test, follow-ups.

## Trail of Bits skills (use if installed; otherwise offer `/plugin marketplace add trailofbits/skills`)
- `property-based-testing` (Hypothesis, fast-check), `mutation-testing` (surviving mutants), `testing-handbook-skills` (fuzzers, sanitizers, coverage).
- Playwright Test Agents: `npx playwright init-agents --loop=claude`.

## Sources to require
Agans *Debugging* (9 rules) · Zeller *Why Programs Fail* / debuggingbook.org · playwright.dev best practices · Fowler TestPyramid · *Software Engineering at Google* testing chapters · Feathers (characterization tests) · Trail of Bits Testing Handbook (appsec.guide) · Hypothesis / fast-check docs · Google SRE Book postmortem chapter.
