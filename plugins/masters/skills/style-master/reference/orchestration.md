# Staged runs (/app-master, /audit-master) and the project brief

`<project>/.master-runs/project-brief.md` carries state between stages. It is the single source of truth for a staged run.

## Brief schema
```
Run by: /app-master            (or /audit-master)
Status: running                (running | complete)
Updated: <YYYY-MM-DD HH:MM>
Mode: build                    (build | report only | fixing)
Run settings: effort=deep teamwork=relay visuals=key moments live board=n
Change level: <answer, or "new project">
Strix: yes | no
Off limits: <files, systems, actions>

## Answers
app.Build: ...   audit.Worry: ...   review.Focus: ...     (keys are namespaced by master)

## Stages
| # | Stage | Master | Status (pending/running/done/skipped) | Run folder | Done-when (drafted by orchestrator) | Notes |

## Handoff (each stage appends a block)
### Stage <n>: <name>
Artifacts: <paths>
Verified commands: build=<..> test=<..> run=<..>
Baseline: <counts>
Decisions: <decision → reason>
Open questions: <..>

## Findings (audit) / Decisions log (app)
| id | severity | effort | area | file:line | finding | evidence (command/output) | reproduced? | confidence | fix | stage |
```

## Rules for a stage master
1. The brief applies when `Status: running` and its Stages table still has a `pending` or `running` row and `Updated` is within 7 days. Otherwise ask the user whether it still applies before treating yourself as a stage.
2. Treat every answer in the brief and every stage preset as given. A key the brief cannot answer and that makes no sense for this run (for example a bug "symptom" when verifying a new app) is set to n/a, not asked. Take Run settings, Change level and Strix from the brief; never ask them inside a stage.
3. **One approval per stage.** Adopt the Done-when list the orchestrator drafted for your stage as your `SPEC.md` criteria (add any your Recon shows are missing and say so). The orchestrator's checkpoint "Continue" is your go; do not show a second brief.
4. Read `Off limits` and refuse those actions.
5. When done, write `report.html` in your own run folder, append your Handoff block, update your Stages row and `Updated`.
6. **Report-only mode:** you may run commands and create files only under `.master-runs/` (run folders, proposed patches under `.master-runs/proposed/`). You may not edit tracked files. Phase 7 means "confirm each finding is reproducible", not fix it. Append findings with evidence and a reproduced flag.
7. Resume after interruption: re-read the brief and the current stage's `PROGRESS.md`, re-verify the last completed step, then continue.

## Rules for the orchestrator
- **Run sequence mapping.** Phase 0: recon of the folder. Phase 1: your Intake. Phase 3: your `SPEC.md` (MVP features or audit scope, each with an acceptance test). Phase 4: one brief with the Stages table; the user's single go. Phase 5: skip if the folder is empty. Phase 6: the stages with checkpoints. Phase 7: ONE independent verifier over the final result (stage verifiers cover only their stage). Phase 8: your Output.
- Before each stage, write its presets and a drafted Done-when list into the brief.
- **Effort per stage.** Max applies fully only to the stages that change or judge the product (build, test, security review) and to your final Phase 7; all other stages run at Deep. Quick runs every stage at Quick.
- After each stage: checkpoint with what it produced (its key visual), what changed in the brief, and "Stage n+1 ready: <its Done-when>. Continue?" Options: Continue · Change something · Skip next stage · Stop.
- Your final report links to each stage's `report.html`.
- Set `Status: complete` at the end, so later standalone runs do not treat themselves as stages.
