# codemap-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Routing overrides
- **Never invoke the `feature-dev` skill** here: it designs and implements. Use only its explorer prompt (`children/feature-dev/agents/code-explorer.md`) or Agent type `feature-dev:code-explorer`, 2 to 3 in parallel with different angles (entry points, data flow, deploy/config).
- **Where is X**: smart-explore (`smart_search`, `smart_outline`). **What calls Y**: the LSP tool (`findReferences`, `incomingCalls`), else `rg -n`. A claim about behaviour needs `smart_unfold` or a Read of that symbol, never an outline alone.
- **learn-codebase** only for repos under about 150 source files or when the user insists; otherwise use the tiered read below. Warn about token cost on large repos.
- **pathfinder** only for "find duplicated concerns / propose a unified architecture". Ask cost first; its output goes to the run folder, not the repo root; skip its `/make-plan` prompts unless asked.
- **what-the** never alone. Read the file or function fully, then answer: Who (callers), What (one sentence), Where (`file:line`), Why it exists (git log, blame or tests), When it runs. Tag each line VERIFIED(`file:line`) or INFERRED. 3 to 5 lines.

## Tiered read (default for whole-repo understanding)
1. README, manifests, CI and deploy config.
2. Tree at depth 2 plus `scc` or `cloc` for size.
3. Entry points.
4. Churn hot spots: `git log --since=1.year --name-only --format= | sort | uniq -c | sort -rn | head -20`, crossed with file size or complexity (Tornhill).
5. Trace one real request end to end.
6. Read in full only the files from steps 3 to 5. List what was not read.

## Output additions
- C4 views: context, container, component; add Deployment and one Dynamic view for the traced request.
- arc42-style sections where relevant: constraints, quality requirements, risks, glossary.
- Data model (ERD from migrations or models), trust boundaries and external entry points.
- Ownership: top authors per area from `git shortlog -sn HEAD -- <dir>` (bus factor).
- "Why" capture: link existing ADRs or decisions found in commits.
- Staleness stamp on any saved doc: commit SHA and date.
- Every claim tagged VERIFIED(`file:line`) or INFERRED.

## Trail of Bits skills (use if installed; otherwise offer `/plugin marketplace add trailofbits/skills`)
`audit-context-building` (function-by-function context) · `trailmark` (code graphs, Mermaid; offer only).

## Gate additions
- At least one request traced end to end with `file:line` hops, verified by running it or a test where possible.

## Sources to require
c4model.com · arc42.org · Nygard ADRs, adr.github.io · Tornhill *Your Code as a Crime Scene* · Feathers (seams) · diataxis.fr · LSP, scc/cloc, dependency-cruiser/madge/pydeps, Structurizr/Mermaid docs.
