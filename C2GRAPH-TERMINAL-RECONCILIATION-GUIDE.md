# Terminal Reconciliation and Successor Admission - v1.3.2

## Resume mode

1. Resolve the durable cursor and exact `run_id`.
2. Read the AgentRun plane. A unique `COMPLETED` record makes same-run resume terminal even if continuity says `open`.
3. If `evaluation_id` exists, require exactly one same-run `kept` evaluation. Contradiction is fail-closed.
4. Run `handoff/validate.py --require-resumable` and default `handoff/terminal_reconcile.py`.

## Successor mode

A different run is not progress by itself. `--successor-json FILE` requires an independently verified predecessor and
`terminal_successor` with predecessor binding, closed class/delta, `basis_ref`, `basis_sha256`, and a canonical
globally unique `basis_claim_id`. If an evaluation ID exists, `predecessor_evaluation_id` must match exactly.

Claim identity is consumed once. Proposal reshaping or copying evidence cannot mint another successor. Basis evidence
may not be direct or symlink-aliased continuity/audit state. The helper is read-only, so the caller must hold the
existing state lock and revalidate immediately before append.
