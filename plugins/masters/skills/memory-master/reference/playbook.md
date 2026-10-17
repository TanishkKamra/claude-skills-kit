# memory-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

**Which tool for which need**
| Need | Use | Note |
|---|---|---|
| "What did we decide or do before?" | mem-search (guided) | stop at the layer that answers |
| Q&A over a topic's history | mem-search first; knowledge-agent only for broad synthesis, then verify | patch below |
| Project story | weekly-digests (long periods) or timeline-report (short) | outputs to the run folder |
| Tokens and cost | agent-cost-report (invoice-like) or session-report (exploration) | state the time zone; dedupe |
| Proof of impact for a manager | receipts | list project names first |
| Memory hygiene, lessons | consolidate-memory, claude-md-improver, caveman-learn | consent per edit |
| Handoff | handoff | |
| Screen history | screenpipe | explicit opt-in only |
| Cloud sync, Telegram modes, CCS alignment, superpowers diagnosis | only on explicit request | cloud-sync uploads memory to a paid cloud: warn first |
standup is a git branch reconciler, not a memory report; use weekly-digests for standups.

**Patches when using children**
- knowledge-agent, timeline-report, weekly-digests: every claim carries observation IDs; verify the 3 most load-bearing claims with `get_observations`; date every recalled fact and flag anything older than 30 days or touching code as "verify against the current repo"; if the corpus is empty, say so.
- timeline-report: confirm before sending more than 30K tokens of history to a subagent.
- session-report: recompute every percentage the model writes from the JSON and fix mismatches.
- All children: write outputs to the run folder (`.master-runs/<run>/`), never into `docs/` or the repo root.
- Write policy for anything saved: no secrets, dedupe before saving, store pointers (paths, IDs) rather than copies, use `decision | reason | date | evidence`.

**Best-of-best sources to require**: Anthropic "Effective context engineering for AI agents" · Anthropic memory tool and context-management docs · Anthropic multi-agent research system post · Claude Code memory docs · MemGPT/Letta · Generative Agents (recency, importance, relevance retrieval) · "Lost in the Middle" · Chroma "Context Rot".
