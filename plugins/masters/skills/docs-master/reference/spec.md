# Spec: agree on "done" before doing anything

Write `SPEC.md` in the run folder after the interview and before the brief. The brief shows its Done-when list; after the user says go, the criteria are frozen (changing one needs the user's yes).

## Template
```
# Spec: <task>
Run settings: effort=<quick|deep|max> teamwork=<solo|relay|panel> visuals=<..> live board=<y/n> change level=<..>
Goal: <one sentence, the outcome the user wants>
Non-goals: <what we will deliberately not do>
Change level: <keep as is | light touch | significant rework | full rebuild>, with the per-decision answers
Constraints: <stack, versions, off-limits files, budget, deadline, accessibility, performance>

## Done when (3 to 8 criteria)
| # | Criterion | Proof (command / file / screenshot / URL) | Expected result |
|---|---|---|---|
| 1 | <observable outcome> | `npm test` | 0 failed (baseline: 3 failed) |
| 2 | <...> | screenshot at 390px | no horizontal scroll |

## Assumptions
- Assumed <X> because <Y>; reversible by <Z>.

## Pre-mortem (imagine this run failed; why?)
| Risk | Likelihood | Guard in the plan |
|---|---|---|
```

## Rules for good criteria
- **Observable and checkable** by running something or opening something, not "code is clean" or "looks good".
- **One proof each**, with the exact command or artifact and the expected output. Prefer automated proofs (tests, build, type-check, lint, link-check, contrast check) over judgment.
- **Include a regression criterion** whenever existing code or content changes: "full test suite: no test that passed at baseline fails".
- **Include an end-to-end criterion** for anything a user touches: drive the real app, open the real file, render the real deck.
- **Taste criteria** (design, writing) get a concrete proxy: "matches reference X in layout and density", "every slide title is a full-sentence claim", "zero placeholder text".
- **No criterion can be satisfied by weakening a test.**
- Quick effort: 2 to 4 criteria and one `Risk:` line instead of the pre-mortem table. Deep: 4 to 8. Max: as many as needed, plus one red-team criterion ("red team finds no confirmed break"; proof: the red-team report in `EVIDENCE.md` with its verdict).

## PROGRESS.md
```
Run settings: effort= teamwork= visuals= live board= change level=
- [x] 0 Recon  - [x] 1 Interview  - [ ] 2 Research ... (the run sequence checklist)
| when | step | action | result | next |
```
Quick runs keep SPEC, PROGRESS and EVIDENCE as three sections of one `RUN.md`.

## Domain examples
- Bug: "repro test `test_x` fails before the fix and passes after"; "full suite: no new failures vs baseline".
- Feature: "acceptance test for each user story passes"; "build + type-check + lint clean".
- UI: "0 axe violations"; "body text contrast ≥ 4.5:1"; "screenshots at 1440 and 390, no overflow"; "all states present: hover, focus, disabled, loading, empty, error".
- Docs/deck: "every number has a source note"; "rendered to PDF, no overflowing text"; "titles alone tell the story".
- Research: "every cited URL opened this run and supports its claim"; "each key claim has 2 independent sources".
- Release: "tag `v1.2.0` exists on remote"; "changelog entry dated today"; "clean-checkout build passes".
- Migration: "no references to the old API: `grep` returns nothing"; "old vs new outputs identical on N samples".
