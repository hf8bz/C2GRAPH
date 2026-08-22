# Progress Liveness and Bounded Self-Audit

C2GRAPH distinguishes **activity**, **diagnostic progress**, and **semantic progress**. Tool calls, heartbeats, reads, cache hits, reviewer output, and checkpoint writes are activity evidence; they do not by themselves reset the semantic-progress clock.

## Classification

The default soft/hard no-semantic-progress thresholds are 7,200 and 28,800 seconds. Earlier diagnosis may be triggered by repeated failure, ratchet exhaustion, checkpoint stagnation, unhealthy tooling, persistent route churn, accounting regression, stale goal state, red gates, or an exceeded declared action deadline. Evidence-backed external waits and declared long-running actions with a valid future deadline remain separate states.

## Strict input boundary - 19 August 2026

Snapshot/finding JSON is validated by **type and value**. JSON booleans are not numbers or integers; numeric strings are rejected at JSON numeric boundaries; integer counters cannot be fractional; numbers must be finite; and deadlines/confidence must satisfy their ranges. Threshold configuration parsing remains a separate boundary because YAML/host configuration may legitimately originate as text.

This closes `C2G-PL-20260819-01`. The observed defect could affect classification/input correctness; no repository-write authority widening path was found.

## Bounded audit and authority

Suspected/hard stagnation may create at most four C2GRAPH read/reason/review diagnostic workers. Findings are advisory, `canonical_write=false`, lane-registered, revision/evidence-bound, and reduced fail-closed. The refutation lane actively attempts to disprove stagnation. Any repository change still requires semantic scope, exact PHASE 3.W `ACTUAL_WRITE_SET`, rollback basis, immediate freshness, and effect verification. BEA, CEA, ACRP, cache/model state, and stagnation severity never become inherited write authority.

## Executed evidence

Current evidence is **32/32 directed scenarios per edition + 65,536/65,536 states** of the declared 16-Boolean abstraction with **0 observed invariant violations**. A separate adversarial lane exercised **40,000 malformed numeric mutations with 0 observed escape**. Exhaustiveness applies only to the declared abstraction.

Primary surfaces: `.agent/progress.schema.md`, `.agent/runbooks/progress-liveness.md`, `scripts/lib/c2progress.py`, `scripts/progress/watch.py`, `scripts/selftest/progress_liveness.py`, and `scripts/verify/progress_liveness.py`.
