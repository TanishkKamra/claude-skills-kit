# Recon: gather context before asking or acting

Goal: know the ground truth before the interview, so you ask less and guess nothing. Write findings to `CONTEXT.md` in the run folder as you go.

## 1. Always read, in this order (skip what does not exist, note that it is missing)
1. The user's message, attached files, links, screenshots. Open every link and file they gave.
2. Project instructions: `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, `CONTRIBUTING.md`, and memory notes about this project.
3. `README` and `docs/` index.
4. Manifest and lockfile: `package.json` + lock, `pyproject.toml` / `requirements*.txt`, `Cargo.toml`, `go.mod`, etc. Record **exact versions** of the main frameworks and libraries.
5. Build, test, lint and type-check config: `tsconfig`, `vite/next/webpack` config, `pytest.ini`, `jest/vitest` config, `.eslintrc`, `ruff`/`black` config. Record the exact commands (`scripts` section, Makefile, task runner).
6. CI config (`.github/workflows/*`), deploy config (`Dockerfile`, `vercel.json`, `fly.toml`, `firebase.json`, `netlify.toml`), env templates (`.env.example`). Never print real secrets from `.env`.
7. Git state: `git status`, current branch, `git log --oneline -15`, uncommitted changes, remotes, `gh auth status` if GitHub matters.
8. The area the task touches: find it with Grep/Glob, then read the files fully, plus their callers and tests.
9. Existing design or product artifacts: `DESIGN.md`, brand files, tokens, screenshots, previous `.master-runs/` reports and `project-brief.md`.

## 2. Probe the environment (cheap commands, read-only)
- Tool versions you will need: `node -v`, `python --version`, `git --version`, `docker --version`, package manager.
- Is the app runnable? Find the dev or start command; note the port. Do not start long-running servers yet, just confirm the command exists.
- Do tests exist and how many? (`ls` test dirs, count test files). Do not run the full suite yet; that is the Baseline phase.

## 3. Scale the effort to the repo
- Small repo or narrow task: read directly.
- Large or unfamiliar repo at Deep/Max: start 2 to 3 read-only explorer subagents in parallel, each with a distinct area (for example frontend, backend/data, build/deploy). Each returns under 300 words with `file:line` evidence: entry points, key modules, conventions, risks. Do not let explorers edit anything.
- Skip generated and vendored code: `node_modules`, `dist`, `build`, `.next`, `vendor`, lockfile bodies.

## 4. Docs and versions (Research phase)
For every library, framework, CLI, API or file format the plan will use and that you did not just read in this repo:
1. Take the exact version from the lockfile or `--version`.
2. Read current docs for that version: context7 (`resolve-library-id` then `query-docs`), the vendor docs site, or the changelog and migration guide.
3. Record in `CONTEXT.md`: library, version, doc URL, the facts you relied on.
4. If docs are unreachable, mark the item "from memory, unverified" and treat it as a risk in the pre-mortem.
For taste-driven work (design, decks, brands), gather 5 to 8 concrete references and say what each is for. Take patterns, never copy assets or text.

## 5. CONTEXT.md layout
```
# Context: <task>
Detected: <one paragraph>
Stack & versions: <lib@version — doc URL>
Commands: build=<..> test=<..> lint=<..> typecheck=<..> run=<..> (verified? y/n)
Relevant files: <path — why>
Conventions: <naming, patterns, style the code already follows>
Existing decisions: <what exists that the user may want to keep>
Missing / could not check: <..>
Risks noticed: <..>
```

## 6. Show the user
One "Detected:" block (5 to 8 lines): what this is, stack and versions, what exists, what is missing, what could not be checked. Then go to the interview and skip every question this already answered.
