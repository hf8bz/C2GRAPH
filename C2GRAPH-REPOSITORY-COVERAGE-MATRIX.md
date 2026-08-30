# C2GRAPH Repository and Capability Coverage Matrix

**Bahasa Indonesia:** [C2GRAPH-REPOSITORY-COVERAGE-MATRIX-ID.md](C2GRAPH-REPOSITORY-COVERAGE-MATRIX-ID.md)

> Scope: public documentation ownership for C2GRAPH v1.2.0. This matrix maps every capability family named by the public README to at least one English and Indonesian public-document owner. It does **not** claim byte-for-byte ownership of every file in the frozen 228-file release tree until the release manifest is reconciled automatically.

## Status vocabulary

- **COVERED** — an existing public guide already owns the capability.
- **NEW** — this documentation expansion adds a public owner.
- **CROSS-CUTTING** — the capability intentionally spans several guides/catalogs.
- **RECONCILE** — the public path/count inventory cannot yet support an exact per-file completeness claim without regenerating from the authoritative release tree.

## 41/41 capability-family ownership

| # | Capability family | Primary English owner | Indonesian owner | Status |
|---:|---|---|---|---|
| 1 | Three-layer context architecture | `C2GRAPH-CONTROL-PLANE-CATALOG.md`, `C2GRAPH-DEEP-DIVE.md` | `C2GRAPH-CONTROL-PLANE-CATALOG-ID.md`, `C2GRAPH-DEEP-DIVE-ID.md` | NEW |
| 2 | Three explicit entry modes | `C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE.md`, `C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md` | `C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE-ID.md`, `C2GRAPH-ENTRY-MODE-DECISION-MATRIX-ID.md` | CROSS-CUTTING |
| 3 | Goal Contract | `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md`, `C2GRAPH-SCHEMA-STATE-CATALOG.md` | corresponding `-ID` files | NEW |
| 4 | Exact Write Plan and rollback authority | `C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md` | `C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE-ID.md` | COVERED |
| 5 | Mutation-free GATE 0 | `C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md` | `C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE-ID.md` | COVERED |
| 6 | Secret-aware discovery | `C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md`, `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md` | corresponding `-ID` files | CROSS-CUTTING |
| 7 | Bounded Evidence Read v2 | `C2GRAPH-BOUNDED-READ-GUIDE.md` | `C2GRAPH-BOUNDED-READ-GUIDE-ID.md` | COVERED |
| 8 | Bounded repository scanners | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md`, `C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md` | corresponding `-ID` files | NEW |
| 9 | Corpus partitions and retrieval discipline | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md`, `C2GRAPH-APPLICATION-LANES-GUIDE.md` | corresponding `-ID` files | NEW |
| 10 | Per-application index boundaries | `C2GRAPH-APPLICATION-LANES-GUIDE.md`, `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md` | corresponding `-ID` files | NEW |
| 11 | Typed Graph Plane | `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md`, `C2GRAPH-SCHEMA-STATE-CATALOG.md` | corresponding `-ID` files | NEW |
| 12 | Progressive query architecture | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md` | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE-ID.md` | NEW |
| 13 | Durable Lessons | `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md`, `C2GRAPH-SCHEMA-STATE-CATALOG.md` | corresponding `-ID` files | NEW |
| 14 | Failure Memory and pre-action recall | `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md`, `C2GRAPH-RUNBOOK-CATALOG.md` | corresponding `-ID` files | NEW |
| 15 | Ratchet loops | `C2GRAPH-RUNBOOK-CATALOG.md`, `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md` | corresponding `-ID` files | NEW |
| 16 | Workflow discipline and rulebook self-checking | `C2GRAPH-CONTROL-PLANE-CATALOG.md`, `C2GRAPH-RUNBOOK-CATALOG.md` | corresponding `-ID` files | NEW |
| 17 | Blocker convergence | `C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md` | `C2GRAPH-BLOCKER-CONVERGENCE-GUIDE-ID.md` | COVERED |
| 18 | Context-compaction continuity | `C2GRAPH-BUDGET-RESUME-GUIDE.md`, `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md` | corresponding `-ID` files | CROSS-CUTTING |
| 19 | Resource-epoch resume | `C2GRAPH-BUDGET-RESUME-GUIDE.md`, `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md` | corresponding `-ID` files | CROSS-CUTTING |
| 20 | Checkpoints as graph lineage | `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md`, `C2GRAPH-SCHEMA-STATE-CATALOG.md` | corresponding `-ID` files | NEW |
| 21 | Tool-usage capture from real call sites | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md`, `C2GRAPH-RUNBOOK-CATALOG.md` | corresponding `-ID` files | NEW |
| 22 | Test-to-workflow mapping | `C2GRAPH-RUNBOOK-CATALOG.md`, `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md` | corresponding `-ID` files | NEW |
| 23 | Requirements provenance | `C2GRAPH-SCHEMA-STATE-CATALOG.md`, `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md` | corresponding `-ID` files | NEW |
| 24 | Human decision boundaries | `C2GRAPH-CONTROL-PLANE-CATALOG.md`, `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md` | corresponding `-ID` files | NEW |
| 25 | Single-agent / multi-agent architecture selection | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md` | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE-ID.md` | NEW |
| 26 | Bounded agent dispatch | `C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md`, `C2GRAPH-RUNBOOK-CATALOG.md` | corresponding `-ID` files | NEW |
| 27 | Runtime-scoped model adapter | `C2GRAPH-MODEL-ADAPTER-GUIDE.md`, `C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md` | corresponding `-ID` files | CROSS-CUTTING |
| 28 | Cache-aware adapter | `C2GRAPH-MODEL-ADAPTER-GUIDE.md` | `C2GRAPH-MODEL-ADAPTER-GUIDE-ID.md` | COVERED |
| 29 | Mobile-specific release governance | `C2GRAPH-APPLICATION-LANES-GUIDE.md`, `C2GRAPH-RUNBOOK-CATALOG.md` | corresponding `-ID` files | NEW |
| 30 | Provenance and W3C PROV-O export | `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md`, `C2GRAPH-SCHEMA-STATE-CATALOG.md` | corresponding `-ID` files | NEW |
| 31 | Compliance evidence mapping | `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md`, `C2GRAPH-CONTROL-PLANE-CATALOG.md` | corresponding `-ID` files | NEW |
| 32 | Centralized configuration | `C2GRAPH-CONFIGURATION-REFERENCE.md` | `C2GRAPH-CONFIGURATION-REFERENCE-ID.md` | NEW |
| 33 | Evidence-first reporting | `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md` | `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE-ID.md` | NEW |
| 34 | Bilingual release surface | `C2GRAPH-PUBLIC-DOCS-INDEX.md`, `README.md` | `C2GRAPH-PUBLIC-DOCS-INDEX-ID.md`, `README-ID.md` | NEW |
| 35 | Bounded Emergency Authorization (BEA v2) | `C2GRAPH-EMERGENCY-AUTHORITY-GUIDE.md` | `C2GRAPH-EMERGENCY-AUTHORITY-GUIDE-ID.md` | COVERED |
| 36 | Adaptive Context Representation Planning (ACRP) | `C2GRAPH-ACRP-GUIDE.md` | `C2GRAPH-ACRP-GUIDE-ID.md` | COVERED |
| 37 | C2GRAPH-native Cognitive Emergency Authorization (CEA) | `C2GRAPH-CEA-GUIDE.md` | `C2GRAPH-CEA-GUIDE-ID.md` | COVERED |
| 38 | Progress Liveness & Bounded Self-Audit | `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-EN.md` | `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-ID.md` | COVERED |
| 39 | Strict runtime-state numeric validation | `C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md`, `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-EN.md` | corresponding ID files | CROSS-CUTTING |
| 40 | Cross-plane authority non-interference | `C2GRAPH-CONTROL-PLANE-CATALOG.md`, `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md` | corresponding `-ID` files | NEW |
| 41 | Handoff event-ledger integrity and authority-gated resume telemetry | `C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md`, `C2GRAPH-SCHEMA-STATE-CATALOG.md`, `C2GRAPH-BUDGET-RESUME-GUIDE.md` | corresponding ID files | NEW |

## Source-family ownership beyond the 41-item README list

The public release inventory also exposes operational families that deserve explicit navigation even when they are implementation mechanisms rather than separate README capability labels:

- discovery cache and invalidation;
- completeness/coverage backfill;
- goal inheritance;
- entity resolution;
- edit tooling;
- browser-tool recovery;
- database migration;
- plan-to-docs;
- post-deploy QA;
- preflight recall;
- propagation of human corrections;
- rule preservation;
- simplification ladder;
- system inventory;
- test-to-workflow mapping;
- tool-usage capture;
- UI audit;
- untrusted-content handling;
- mobile build/release, device QA, and store submission;
- monorepo index placement.

These are owned by the Runbook Catalog, Application Lanes Guide, Agent/Discovery/Orchestration Guide, Script/Verification Catalog, and their Indonesian companions.

## Remaining exact-inventory blockers

This matrix supports **41/41 capability-family public documentation ownership after this package is merged**. It intentionally does not claim exact `228/228` source-path ownership because the current public File Map reports totals that are not self-reconciling from its own path tables:

- `40` runbooks including the template versus `36` named Markdown runbook paths visible in the File Map;
- `64` Python scripts while the File Map's script table is not itself an obvious exhaustive 64-path catalog.

Do not invent missing paths. Regenerate catalogs from `.agent/distribution-files.txt` and the same source measurement used by `scripts/verify/counts.py --json`, then make count/catalog mismatch a verification failure.

## Completion criterion

Documentation completeness has two independent levels:

1. **Capability completeness:** every public capability has an English and Indonesian explanatory owner — this package targets 41/41.
2. **Path completeness:** every shipped non-internal file/path maps to a public owner — this remains `RECONCILE` until generated from the authoritative release tree.
