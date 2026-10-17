# ops-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Routing overrides
- **System map** (first step): read Dockerfile, CI, IaC and config directly, or use codemap-master's explorer prompt / `caveman:cavecrew-investigator`. **investigate-first** only diagnoses failures; it does not map.
- **This master runs the checklist itself**; no child owns it. Emit the scorecard from EVIDENCE.md.
- **torture-test** is correctness and failure fuzzing for local apps, not a load test. Round 1 only; Round 2 (DAC, sleep, hardware) only for Cadence. Max 4 agents; pass it the SPEC.md request and duration cap.
- **Load tests**: k6 (smoke, load, stress, soak, spike) with thresholds and abort criteria, local only (isolated database, mocked paid services) within the request cap; staging or any other host needs a yes naming the host, and never a site the user does not own. Use the k6 skills if installed (`k6-test-planner`, `k6-load-test`, `k6-smoke-test`, `k6-browser-test`, `k6-docs`) and the `k6` MCP server for script validation; otherwise offer `k6 x agent init claude-code` and ask before installing. Never use Grafana Cloud k6 without an explicit request.
- **webapp-testing** proves function only. Measure timings with Lighthouse or k6 browser; never use `networkidle`; write artifacts to the run folder.
- **childhood**: generic post-release watch; ignore its claude-mem sections and dead links.
- **verification-before-completion** owns claims; EVIDENCE.md owns format.

## Measurement rigour
Every measurement: warm-up discarded, n of at least 5 runs, same hardware and load profile; report p50, p95, p99 and error rate before and after, never the mean alone. Report USE (utilisation, saturation, errors) for each resource and RED (rate, errors, duration) for each service.

## Checklist additions (append to the master's 16 items)
17. Security baseline [all]: HTTPS and HSTS, security headers (`npx @mdn/mdn-http-observatory <host>`), dependency and secret scan, rate limits on auth.
18. Graceful shutdown [all]: SIGTERM drains in-flight work.
19. RPO and RTO written, with a restore drill [growing].
20. Certificate and domain expiry alerts [all].
21. Capacity: saturation headroom and load shedding [growing].
22. Database [if any]: EXPLAIN on the slowest queries, N+1 check, pool sizing.
23. Timeouts, retries with backoff and jitter on every outbound call [all].
24. Runbook and incident/postmortem template [growing].

Alerting: once SLOs exist, use multi-window burn-rate alerts (page on fast burn, ticket on slow burn).

## Optional tools (offer, never auto-install; ask for each)
k6 / grafana-k6 · Sentry plugin or MCP · MDN HTTP Observatory · built-in `/security-review` · Trail of Bits `static-analysis` · Trivy, gitleaks, Toxiproxy, restic/pgBackRest, OpenTelemetry, Lighthouse CI (check install lines first).

## Gate additions
- Every before/after number has n, percentiles, hardware and load profile.
- Scorecard covers all applicable checklist items, each PASS / FAIL / N/A with evidence.

## Sources to require
Google SRE Book (ch. 4 SLOs, ch. 6 monitoring, ch. 27 launch checklist) · SRE Workbook (alerting on SLOs) · OWASP Top 10 and ASVS · Brendan Gregg USE method · Tom Wilkie RED method · AWS Builders' Library (timeouts, retries, jitter) · Grafana k6 docs · PostgreSQL EXPLAIN and pooling docs · Twelve-Factor App · DORA metrics.
