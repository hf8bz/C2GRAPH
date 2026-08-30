# Dokumentasi C2GRAPH — Mulai Di Sini

> Router dokumentasi publik C2GRAPH v1.2.0. Halaman ini membantu memilih dokumen; ia tidak mengganti authority dari prompt executable, `AGENTS.md`, routes, scopes, runbooks, config, schema, atau runtime state.

**English:** [C2GRAPH-DOCS-START-HERE.md](C2GRAPH-DOCS-START-HERE.md)

## 1. Pilih formal entry mode

C2GRAPH hanya memiliki tiga formal entry mode:

| Kondisi repository | Formal mode | Entry point |
|---|---|---|
| Existing non-empty repo + objective implementasi nyata | `ADOPT_WITH_GOAL` | `SCAN-AND-ADOPT.prompt.md` |
| Existing non-empty repo + hanya install/repair/upgrade/verify C2GRAPH | `WORKFLOW_ONLY` | `INSTALL-WORKFLOW-ONLY.prompt.md` |
| Empty/new repo | `BOOTSTRAP` | `BOOTSTRAP-NEW-REPO.prompt.md` |

`SCAN` **bukan** mode keempat. `SCAN-AND-ADOPT.prompt.md` adalah nama prompt untuk direct `ADOPT_WITH_GOAL` path.

## 2. Pilih dokumen sesuai pertanyaan

- overview dan prinsip → `README-ID.md`;
- arsitektur → `C2GRAPH-DEEP-DIVE-ID.md`;
- rules/gates/thresholds → `C2GRAPH-REFERENCE-ID.md`;
- semua public docs → `C2GRAPH-PUBLIC-DOCS-INDEX-ID.md`;
- 41 capability dan owner-nya → `C2GRAPH-REPOSITORY-COVERAGE-MATRIX-ID.md`;
- control/config/runbook/schema/prompt/script inventory → catalog terkait;
- discovery, application lane, graph lifecycle, evidence/provenance → cross-cutting guide terkait.

## 3. Focused operational guides

Gunakan guide khusus untuk Bounded Read, exact write/rollback, GATE 0, secret safety, blockers, budget/resume, model adapter, BEA, ACRP, CEA, Progress Liveness, CLI launcher, workflow-only, atau troubleshooting. Semua pasangan EN/ID tercantum di Public Docs Index.

## 4. Verification & assurance

Evidence engineering dipublikasikan melalui Script & Verification Catalog, Evidence/Provenance/Assurance Guide, Reference, dan frozen v1.2.0 Release Notes.

Paper/research metadata tetap berada pada README/CITATION dan tidak dicampur dengan operator authority.

## 5. Frozen release versus current docs

File Map dapat mendeskripsikan frozen v1.2.0 bundle, sedangkan public repo dapat menerima explanatory docs setelah release. Jangan mengubah historical frozen count hanya agar sama dengan current public-doc count.
