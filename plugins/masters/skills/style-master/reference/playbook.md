# style-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Effort
Default this master to **Quick** unless the user picks otherwise: the Deep protocol (critic, verifier) can cost more tokens than reply styling saves.

## Level mapping
- Lite = caveman with only rules 1, 2 and 5 (keep articles and full sentences).
- Full = caveman. Ultra = ultracave. Max = ultracave.
- **megacave** only if the user reads Classical Chinese and names it.
- Search = exploration only: use the `caveman:cavecrew-investigator` agent. caveman-explore is an agent definition, not a skill; it works only if copied to `~/.claude/agents/`.
- **caveman-setup and caveman-optimize: never route.** They send all LLM traffic through a third-party gateway. Only on an explicit request plus a fresh yes, with that warning.
- cavecrew: use `pr-review-toolkit:code-reviewer` where it mentions a vanilla "Code Reviewer".

## Bigger levers first (Claude Code costs docs)
Before reply styling, check and offer: `/clear` between tasks; `/compact` with a focus; `/context` to find bloat; prune unused MCP servers and plugins (tool definitions cost every turn); `/doctor` for unused skills and a bloated CLAUDE.md; keep CLAUDE.md under 200 lines and move rare sections to skills or `.claude/rules/`; the smallest adequate model and effort per task; delegate verbose work to subagents; keep prompt-cache hits (do not switch model mid-session).

## Patches when using children
- **caveman-compress**: tell the user first that file text is sent to the model provider. For "Show a diff first" or "Dry run", run on a copy and show the diff before replacing. Measure that instructions still work after compressing.
- **caveman-commit**: match the repo convention (`git log -20 --oneline`); keep any required `Co-Authored-By` trailer.
- **full-output-enforcement**: applies only to files and deliverables the user asked for in full; never to chat explanations, diffs or edits.

## Measurement (replaces the token-count gate)
- Reply modes: savings are not measurable without a baseline; report `unknown` unless an A/B on the same task was run.
- Files: bytes plus an exact token count before and after (Anthropic `count_tokens` API, or `claude -p --output-format json` usage).
- Quality guard: after any compression, run one representative task and confirm the answer is still correct.

## Sources to require
Claude Code docs "Manage costs effectively" and memory docs · Anthropic prompting best practices · Anthropic prompt caching and token counting docs · ASD-STE100 · Anthropic "Effective context engineering for AI agents".
