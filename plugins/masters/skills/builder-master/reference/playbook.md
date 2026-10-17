# builder-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Precedence when children disagree
1. Current docs at code.claude.com (skills, hooks, sub-agents, plugins-reference, mcp, memory) beat any CHILD.md. Fetch them in Phase 2; children are snapshots.
2. **writing-skills** owns test-before-deploy. Its "description = when only" rule is overridden by the description rule below.
3. **skill-development** supplies layout only; its word counts yield to the 500-line cap.
4. **mcp-builder** owns tool design; MCP security comes from modelcontextprotocol.io security best practices.
5. **command-development** is legacy: for new commands write a skill.
6. Prefer `anthropic-skills:skill-creator` (eval and description-optimisation loop) when installed.
7. caveman-discover and caveman-manage are Caveman Cloud tools: only if the user names them.

## Known child errors to correct
- **hook-development**: the event list is stale (about 30 events now) and handler types are command, http, mcp_tool, prompt and agent; check the live list at code.claude.com/docs/en/hooks. Its deny examples are wrong: exit 2 uses the stderr text as the reason and ignores JSON. Block with `echo "Blocked: <reason>" >&2; exit 2`, or for structured control exit 0 with stdout `{"hookSpecificOutput":{"hookEventName":"PreToolUse","permissionDecision":"deny","permissionDecisionReason":"..."}}`. On Windows add `"shell": "powershell"` or ship a PowerShell script.
- **agent-development**: only `name` and `description` are required; colours are red, blue, green, yellow, purple, orange, pink, cyan. Always set `tools` or `disallowedTools`: omitted means it inherits everything. Plugin subagents ignore hooks, mcpServers and permissionMode.
- **mcp-integration**: use `type: http`; SSE is deprecated.
- **mcp-builder** step 1.4: start from the 5 to 10 user workflows, not endpoint coverage.
- **plugin-settings**: prefer plugin.json `userConfig` (sensitive values stay out of files) and `${CLAUDE_PLUGIN_DATA}`.
- **claude-md-improver**: the file is `CLAUDE.local.md`; target under 200 lines; use `.claude/rules/` and `@imports`; do not reward bloat.

## Rules
- **Description**: third person; what + when + when-not; key use case first; description + `when_to_use` together at most 1,536 characters. Add `disable-model-invocation: true` for side-effect skills. Never summarise the workflow steps in the description.
- **MCP security checklist**: no token passthrough; validate token audience; per-client consent for OAuth proxies; SSRF-safe fetches; minimum scopes; Origin check and localhost binding for local HTTP; no secrets in logs.
- **Shell-capable hooks, skills and servers**: review for injection (quote variables), secret handling and least privilege.
- **Currency check**: diff each child claim you rely on against the live docs; note mismatches in CONTEXT.md.
- **Distribution**: marketplace.json, version bump, changelog, `userConfig` for secrets.
- **Windows**: test hook scripts on PowerShell and Git Bash.

## Gate additions
- `claude plugin validate <dir>` output pasted.
- Trigger eval: 5 should-fire and 5 near-miss prompts; report the rate. `claude plugin eval` when available.

## Sources to require
code.claude.com docs (skills, hooks, sub-agents, plugins-reference, mcp, memory) · Anthropic "Skill authoring best practices" (refetch) · modelcontextprotocol.io specification and security best practices · MCP authorization spec + RFC 9700 · anthropics/skills (skill-creator, mcp-builder) · claude-plugins-official plugin-dev · OWASP Top 10 for LLM Applications · trailofbits/skills as a quality reference.
