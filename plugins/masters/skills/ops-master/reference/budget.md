# Token budget: deliver the best result without running out mid-task

Usage limits (5-hour window and weekly, shared across Claude and Claude Code and across models) are not published as fixed numbers, so read live data instead of guessing.

## Before starting (Phase 1 and the brief)
1. Check usage where possible: `/usage` (plan bars and what used them) or `/status`. If you cannot read it, ask the user once: "Plenty of usage left, or should I keep this lean?"
2. Give a **cost range** in the brief, labelled as an estimate: roughly the number of agents × their size, plus main-thread turns × context size. Quick ≈ 1×, Deep ≈ 2 to 3×, Max ≈ 5 to 10×.
3. If usage is above about 70% of the window, recommend a lower effort or splitting the task into stages, and ask before anything heavy.

## While working
- **Model per job:** read-only exploration, fetching and searching subagents use `model: haiku` or `sonnet`; keep the strongest model for planning, judging and the final verifier.
- **Lean delegations:** focused prompts, file paths instead of file contents, and short structured returns (under 300 words) so results do not bloat the main context.
- **Fan out only for independent, read-only work.** Cap parallel agents at 3 (Max: 4) unless the user asks for more. Never start agent teams or background loops unasked.
- **Do not switch model or effort mid-run**; each switch breaks the prompt cache.
- **Checkpoint at every stage**: write state to `PROGRESS.md`, then continue. Compact at stage boundaries, never mid-step.
- **Stop and ask** before launching new agents if usage crosses the threshold the user gave (default 85%): continue, finish at a lower effort, or pause.

## If a limit is hit
- `PROGRESS.md`, `SPEC.md` and `EVIDENCE.md` hold everything needed to resume; a fresh session re-reads them and re-verifies the last completed step, never repeating finished work.
- In an interactive subscription session Claude Code can wait and auto-continue after the reset; do not resend the request.
- A weekly limit may be days away: tell the user exactly where the run stopped and the one command to resume (`/resume`, or re-run the master, which finds the run folder).
