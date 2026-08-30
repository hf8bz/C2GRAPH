# Matriks Coverage Repository dan Capability C2GRAPH

**English:** [C2GRAPH-REPOSITORY-COVERAGE-MATRIX.md](C2GRAPH-REPOSITORY-COVERAGE-MATRIX.md)

> Scope: ownership dokumentasi publik C2GRAPH v1.2.0. Matriks ini membuktikan **capability-level documentation ownership**, bukan exact per-path completeness pada frozen 228-file release tree.

## 41/41 capability family

| # | Capability family | Owner dokumentasi publik EN/ID | Status |
|---:|---|---|---|
| 1 | Three-layer context architecture | Control Plane Catalog + Deep Dive | **TERCAKUP setelah overlay** |
| 2 | Three explicit entry modes | Install/Adopt/Bootstrap + Entry Mode Matrix | **TERCAKUP setelah overlay** |
| 3 | Goal Contract | Graph/State Lifecycle + Schema/State Catalog | **TERCAKUP setelah overlay** |
| 4 | Exact Write Plan and rollback authority | Actual Write Plan & Rollback Guide | **TERCAKUP setelah overlay** |
| 5 | Mutation-free GATE 0 | Safe GATE 0 Guide | **TERCAKUP setelah overlay** |
| 6 | Secret-aware discovery | Secret Safety + Agent/Discovery Guide | **TERCAKUP setelah overlay** |
| 7 | Bounded Evidence Read v2 | Bounded Read Guide | **TERCAKUP setelah overlay** |
| 8 | Bounded repository scanners | Agent/Discovery + Script/Verification Catalog | **TERCAKUP setelah overlay** |
| 9 | Corpus partitions and retrieval discipline | Agent/Discovery + Application Lanes | **TERCAKUP setelah overlay** |
| 10 | Per-application index boundaries | Application Lanes + Agent/Discovery | **TERCAKUP setelah overlay** |
| 11 | Typed Graph Plane | Graph/State Lifecycle + Schema/State Catalog | **TERCAKUP setelah overlay** |
| 12 | Progressive query architecture | Agent/Discovery Guide | **TERCAKUP setelah overlay** |
| 13 | Durable Lessons | Graph/State Lifecycle + Schema/State Catalog | **TERCAKUP setelah overlay** |
| 14 | Failure Memory and pre-action recall | Graph/State Lifecycle + Runbook Catalog | **TERCAKUP setelah overlay** |
| 15 | Ratchet loops | Runbook Catalog + Evidence/Assurance | **TERCAKUP setelah overlay** |
| 16 | Workflow discipline and rulebook self-checking | Control Plane + Runbook Catalog | **TERCAKUP setelah overlay** |
| 17 | Blocker convergence | Blocker Convergence Guide | **TERCAKUP setelah overlay** |
| 18 | Context-compaction continuity | Budget/Resume + Graph/State Lifecycle | **TERCAKUP setelah overlay** |
| 19 | Resource-epoch resume | Budget/Resume + Graph/State Lifecycle | **TERCAKUP setelah overlay** |
| 20 | Checkpoints as graph lineage | Graph/State Lifecycle + Schema/State Catalog | **TERCAKUP setelah overlay** |
| 21 | Tool-usage capture from real call sites | Agent/Discovery + Runbook Catalog | **TERCAKUP setelah overlay** |
| 22 | Test-to-workflow mapping | Runbook Catalog + Evidence/Assurance | **TERCAKUP setelah overlay** |
| 23 | Requirements provenance | Schema/State + Evidence/Assurance | **TERCAKUP setelah overlay** |
| 24 | Human decision boundaries | Control Plane + Evidence/Assurance | **TERCAKUP setelah overlay** |
| 25 | Single-agent / multi-agent architecture selection | Agent/Discovery Guide | **TERCAKUP setelah overlay** |
| 26 | Bounded agent dispatch | Agent/Discovery + Runbook Catalog | **TERCAKUP setelah overlay** |
| 27 | Runtime-scoped model adapter | Model Adapter + Script/Verification | **TERCAKUP setelah overlay** |
| 28 | Cache-aware adapter | Model Adapter Guide | **TERCAKUP setelah overlay** |
| 29 | Mobile-specific release governance | Application Lanes + Runbook Catalog | **TERCAKUP setelah overlay** |
| 30 | Provenance and W3C PROV-O export | Evidence/Assurance + Schema/State | **TERCAKUP setelah overlay** |
| 31 | Compliance evidence mapping | Evidence/Assurance + Control Plane | **TERCAKUP setelah overlay** |
| 32 | Centralized configuration | Configuration Reference | **TERCAKUP setelah overlay** |
| 33 | Evidence-first reporting | Evidence/Assurance Guide | **TERCAKUP setelah overlay** |
| 34 | Bilingual release surface | Public Docs Index + README/README-ID | **TERCAKUP setelah overlay** |
| 35 | Bounded Emergency Authorization (BEA v2) | Emergency Authority Guide EN/ID | **TERCAKUP setelah overlay** |
| 36 | Adaptive Context Representation Planning (ACRP) | ACRP Guide EN/ID | **TERCAKUP setelah overlay** |
| 37 | C2GRAPH-native Cognitive Emergency Authorization (CEA) | CEA Guide EN/ID | **TERCAKUP setelah overlay** |
| 38 | Progress Liveness & Bounded Self-Audit | Progress Liveness EN/ID | **TERCAKUP setelah overlay** |
| 39 | Strict runtime-state numeric validation | Script/Verification + Liveness | **TERCAKUP setelah overlay** |
| 40 | Cross-plane authority non-interference | Control Plane + Evidence/Assurance | **TERCAKUP setelah overlay** |
| 41 | Handoff event-ledger integrity and authority-gated resume telemetry | Graph/State + Schema/State + Budget/Resume | **TERCAKUP setelah overlay** |

## Surface operasional tambahan

Selain 41 capability family README, File Map mengekspos mekanisme seperti discovery cache, coverage backfill, goal inheritance, entity resolution, browser-tool recovery, edit tooling, database migration, plan-to-docs, post-deploy QA, preflight recall, propagation, rule preservation, simplification ladder, system inventory, test-to-workflow, tool-usage capture, UI audit, untrusted-content, mobile release runbooks, dan monorepo index placement. Surface ini dimiliki oleh Runbook Catalog, Application Lanes, Agent/Discovery/Orchestration, Script/Verification Catalog, serta companion ID-nya.

## Batas completeness

Setelah overlay di-merge, **41/41 capability family memiliki owner EN dan ID**. Namun exact `228/228` source-path coverage belum boleh diklaim sampai public inventory direkonsiliasi dari authoritative tree/manifest karena:

- File Map melaporkan 40 runbook termasuk template tetapi terlihat mengenumerasi 36 Markdown runbook bernama;
- File Map melaporkan 64 Python scripts tetapi tabel path-nya tidak menjadi obvious exact 64-path catalog.

Missing path tidak boleh dibuat berdasarkan tebakan. Generate dari `.agent/distribution-files.txt` / source tree yang sama dengan `scripts/verify/counts.py --json`, lalu fail verification jika count dan catalog berbeda.
