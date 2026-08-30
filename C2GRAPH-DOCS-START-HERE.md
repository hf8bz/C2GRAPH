# C2GRAPH Documentation — Start Here

> C2GRAPH v1.2.0 public documentation router. This page explains where to read next. It does not override executable prompts, `AGENTS.md`, routes, scopes, runbooks, configuration, schemas, or exact runtime state.

**Bahasa Indonesia:** [C2GRAPH-DOCS-START-HERE-ID.md](C2GRAPH-DOCS-START-HERE-ID.md)

## 1. Choose the formal entry mode first

Do not use `SCAN` as a fourth mode. `SCAN-AND-ADOPT.prompt.md` is the executable prompt name for the formal `ADOPT_WITH_GOAL` path.

| Repository condition | Formal mode | Start with |
|---|---|---|
| Existing non-empty repository + real implementation objective | `ADOPT_WITH_GOAL` | `SCAN-AND-ADOPT.prompt.md` + [Entry Mode Matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) |
| Existing non-empty repository + C2GRAPH install/repair/upgrade/verification only | `WORKFLOW_ONLY` | `INSTALL-WORKFLOW-ONLY.prompt.md` + [Workflow-Only Guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) |
| Empty or new repository | `BOOTSTRAP` | `BOOTSTRAP-NEW-REPO.prompt.md` + [Entry Mode Matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) |

For the lifecycle across all three modes, read [Install, Adopt & Bootstrap Guide](C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE.md).

## 2. Choose documentation by question

- “What is C2GRAPH and why does it exist?” → [README](README.md)
- “How does the architecture fit together?” → [Deep Dive](C2GRAPH-DEEP-DIVE.md)
- “What is the exact rule/gate/config/state contract?” → [Reference](C2GRAPH-REFERENCE.md)
- “What public docs exist?” → [Public Docs Index](C2GRAPH-PUBLIC-DOCS-INDEX.md)
- “Which capability is documented where?” → [Repository Coverage Matrix](C2GRAPH-REPOSITORY-COVERAGE-MATRIX.md)
- “Which runbook/prompt/schema/script/control surface owns this?” → the public catalogs in the [Public Docs Index](C2GRAPH-PUBLIC-DOCS-INDEX.md)
- “How do discovery, graph state, provenance, application lanes, or verification work across subsystems?” → the cross-cutting guides in the Public Docs Index.

## 3. Focused operational guides

Use the focused guide when your problem is already localized:

- bounded/minified/log evidence → `C2GRAPH-BOUNDED-READ-GUIDE.md`;
- exact writes and rollback → `C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md`;
- mutation-free GATE 0 / rollback preconditions → `C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md`;
- secrets → `C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md`;
- blockers → `C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md`;
- context/resource exhaustion → `C2GRAPH-BUDGET-RESUME-GUIDE.md`;
- model/cache/tool runtime → `C2GRAPH-MODEL-ADAPTER-GUIDE.md`;
- BEA → `C2GRAPH-EMERGENCY-AUTHORITY-GUIDE.md`;
- ACRP → `C2GRAPH-ACRP-GUIDE.md`;
- CEA → `C2GRAPH-CEA-GUIDE.md`;
- progress/liveness → `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-EN.md`;
- CLI launcher notation → `C2GRAPH-CLI-LAUNCHER-GUIDE.md`;
- stopped/inconsistent run → `C2GRAPH-TROUBLESHOOTING.md`.

Every item above has an Indonesian public companion in `C2GRAPH-PUBLIC-DOCS-INDEX-ID.md` after this documentation overlay is merged.

## 4. Verification and assurance

Verification evidence is documented through the engineering verification/assurance surfaces below:

- `C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md` for verifier/script surfaces;
- `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md` for evidence classes, provenance, claim boundaries and compliance mapping;
- the v1.2.0 Release Notes for the frozen release verification snapshot.

Research paper metadata remains in the README/CITATION surface; it is independent from operator documentation.

## 5. Frozen release inventory versus current public docs

`C2GRAPH-FILE-MAP.md` describes the frozen v1.2.0 release inventory. The public repository may receive explanatory documentation after the frozen release. Do not silently rewrite a frozen release count to equal the current public-doc count.

Where a public inventory total and its visible path list cannot be reconciled, regenerate from the authoritative release tree/manifest rather than inventing missing paths.

## 6. Authority boundary

Public documentation explains C2GRAPH. It does not manufacture repository-write authority. Effective mutation still depends on the governing source contracts, including semantic scope, exact `ACTUAL_WRITE_SET`, rollback basis, fresh pre-state, and verification.
