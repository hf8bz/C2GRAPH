# C2GRAPH v1.3.2 Terminal Reconciliation Release Overview

Release date: 3 September 2026

C2GRAPH v1.3.2 is a backward-compatible corrective release over the v1.3.1 package. It keeps CORE-40 Workflow
Convergence and adds a cross-plane terminal boundary so stale continuity cannot reopen a `COMPLETED` AgentRun.
Resume and terminal-successor admission are separate executable modes.

## New terminal boundary

- `COMPLETED` outranks stale `open` CURSOR/HANDOFF state for same-run resume.
- When a predecessor carries `evaluation_id`, exactly one same-run `kept` evaluation is required for verified close.
- A contradictory/non-kept evaluation fails closed even if `used_as_evidence` is true.
- A successor requires `terminal_successor`, an exact non-control-plane evidence digest, and a globally unique canonical
  `basis_claim_id` that occurs as an exact token in UTF-8 evidence.
- A claim is consumed once across the handoff ledger. Changing class, semantic delta, evidence path, copied evidence,
  model, reviewer, session, or run/task ID does not mint a second successor.
- Direct or symlink-aliased basis evidence from `.agent/state/**`, `.agent/runs/**`, `.agent/graph/**`, or `backups/**`
  is rejected.
- The admission helper is read-only. Writers still need the existing state lock/fence and must revalidate immediately
  before checkpoint append. No repository-write authority is added.

## Measured source identity

Per language edition: 239 shipped/distribution files, 72 Python scripts, 41 shipped validators, 43 registered checks,
40 contiguous CORE rules, 46 routing rows, 41 runbooks including template, and 28 graph-engineering documents.

## Verification boundary

The release audit reruns the full EN and ID verifier from fresh package extraction and separately exercises terminal
state matrices, successor mutations, claim-reuse chains, handoff projection, package parity, and upgrade structure.
Evidence is bounded falsification, not a mathematical proof that defects are impossible under every future host,
filesystem crash, or external concurrent writer.

## Publication identity

The v1.2.0 release remains immutable history. C2GRAPH v1.3.2 is published with software DOI
`10.5281/zenodo.22259056`; the bilingual v1.3.2 release paper is published with DOI
`10.5281/zenodo.22259065`.
