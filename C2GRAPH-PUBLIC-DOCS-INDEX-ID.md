# Indeks Dokumentasi Publik C2GRAPH

> Router dokumentasi publik C2GRAPH v1.2.0. Dokumentasi menjelaskan sistem, tetapi otoritas eksekusi tetap berasal dari prompt executable, `AGENTS.md`, routes, scopes, runbooks, konfigurasi, schema, exact runtime state, dan verification scripts yang dikirim bersama C2GRAPH.

**English:** [C2GRAPH-PUBLIC-DOCS-INDEX.md](C2GRAPH-PUBLIC-DOCS-INDEX.md)

## Kebijakan bahasa

Permukaan dokumentasi publik dipelihara sebagai pasangan semantik English + Bahasa Indonesia.

- Nama file English tanpa suffix tetap dipertahankan untuk kompatibilitas pada sebagian besar dokumen.
- Companion Bahasa Indonesia menggunakan `-ID.md`.
- Dokumen Progress Liveness yang sejak awal memakai `-EN.md` / `-ID.md` tetap menggunakan konvensi tersebut.
- Path, identifier, nama gate, nama field schema, token kode, serta command **tidak diterjemahkan**.
- Companion Bahasa Indonesia tidak boleh memperluas authority atau menciptakan perilaku yang tidak ada pada source contract executable.

## Mulai dan pilih entry mode

| Topik | English | Bahasa Indonesia |
|---|---|---|
| Overview repository | [README](README.md) | [README Indonesia](README-ID.md) |
| Router dokumentasi | [Docs Start Here](C2GRAPH-DOCS-START-HERE.md) | [Mulai Dokumentasi](C2GRAPH-DOCS-START-HERE-ID.md) |
| Pemilihan entry mode | [Entry Mode Decision Matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) | [Matriks Mode Masuk](C2GRAPH-ENTRY-MODE-DECISION-MATRIX-ID.md) |
| Siklus install/adopt/bootstrap | [Install, Adopt & Bootstrap](C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE.md) | [Instal, Adopsi & Bootstrap](C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE-ID.md) |
| Workflow-only | [Workflow-Only Guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) | [Panduan Workflow-Only](C2GRAPH-WORKFLOW-ONLY-GUIDE-ID.md) |
| Instruksi workflow-only untuk agent | [Agent Instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) | [Instruksi Agent](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION-ID.md) |

## Arsitektur, authority, dan state

| Topik | English | Bahasa Indonesia |
|---|---|---|
| Arsitektur mendalam | [Deep Dive](C2GRAPH-DEEP-DIVE.md) | [Deep Dive Indonesia](C2GRAPH-DEEP-DIVE-ID.md) |
| Rules/gates/thresholds | [Reference](C2GRAPH-REFERENCE.md) | [Referensi Indonesia](C2GRAPH-REFERENCE-ID.md) |
| Topologi control plane | [Control Plane Catalog](C2GRAPH-CONTROL-PLANE-CATALOG.md) | [Katalog Control Plane](C2GRAPH-CONTROL-PLANE-CATALOG-ID.md) |
| Siklus graph/work-state | [Graph & State Lifecycle](C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md) | [Siklus Graph & State](C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE-ID.md) |
| Schema dan runtime state | [Schema & State Catalog](C2GRAPH-SCHEMA-STATE-CATALOG.md) | [Katalog Schema & State](C2GRAPH-SCHEMA-STATE-CATALOG-ID.md) |
| Konfigurasi terpusat | [Configuration Reference](C2GRAPH-CONFIGURATION-REFERENCE.md) | [Referensi Konfigurasi](C2GRAPH-CONFIGURATION-REFERENCE-ID.md) |

## Evidence, discovery, dan execution safety

| Topik | English | Bahasa Indonesia |
|---|---|---|
| Bounded Evidence Read | [Bounded Read](C2GRAPH-BOUNDED-READ-GUIDE.md) | [Bounded Read Indonesia](C2GRAPH-BOUNDED-READ-GUIDE-ID.md) |
| Discovery/indexing/agent dispatch | [Agent, Discovery & Orchestration](C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md) | [Agent, Discovery & Orchestration ID](C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE-ID.md) |
| Application/scope lanes | [Application Lanes](C2GRAPH-APPLICATION-LANES-GUIDE.md) | [Application Lanes Indonesia](C2GRAPH-APPLICATION-LANES-GUIDE-ID.md) |
| Evidence/provenance/assurance | [Evidence, Provenance & Assurance](C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE.md) | [Evidence, Provenance & Assurance ID](C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE-ID.md) |
| Exact writes + rollback | [Actual Write Plan & Rollback](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md) | [Versi Indonesia](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE-ID.md) |
| GATE 0 mutation-free | [Safe GATE 0](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) | [Versi Indonesia](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE-ID.md) |
| Secret-prone path | [Secret Safety](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md) | [Versi Indonesia](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE-ID.md) |

## Runtime, adaptive controls, dan continuity

| Topik | English | Bahasa Indonesia |
|---|---|---|
| Model/cache/tool runtime | [Model Adapter](C2GRAPH-MODEL-ADAPTER-GUIDE.md) | [Model Adapter Indonesia](C2GRAPH-MODEL-ADAPTER-GUIDE-ID.md) |
| Emergency authority | [BEA v2](C2GRAPH-EMERGENCY-AUTHORITY-GUIDE.md) | [BEA v2 Indonesia](C2GRAPH-EMERGENCY-AUTHORITY-GUIDE-ID.md) |
| Adaptive context representation | [ACRP](C2GRAPH-ACRP-GUIDE.md) | [ACRP Indonesia](C2GRAPH-ACRP-GUIDE-ID.md) |
| Cognitive emergency containment | [CEA](C2GRAPH-CEA-GUIDE.md) | [CEA Indonesia](C2GRAPH-CEA-GUIDE-ID.md) |
| Progress liveness/self-audit | [Liveness EN](C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-EN.md) | [Liveness ID](C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-ID.md) |
| Resource/context resume | [Budget & Resume](C2GRAPH-BUDGET-RESUME-GUIDE.md) | [Budget & Resume Indonesia](C2GRAPH-BUDGET-RESUME-GUIDE-ID.md) |
| Blocker convergence | [Blocker Convergence](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md) | [Blocker Convergence Indonesia](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE-ID.md) |

## Catalog, coverage, dan operasi

| Topik | English | Bahasa Indonesia |
|---|---|---|
| Inventory runbook | [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md) | [Katalog Runbook](C2GRAPH-RUNBOOK-CATALOG-ID.md) |
| Prompt surfaces | [Prompt Catalog](C2GRAPH-PROMPT-CATALOG.md) | [Katalog Prompt](C2GRAPH-PROMPT-CATALOG-ID.md) |
| Script + verification | [Script & Verification Catalog](C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md) | [Katalog Script & Verification](C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md) |
| Ownership capability/source | [Repository Coverage Matrix](C2GRAPH-REPOSITORY-COVERAGE-MATRIX.md) | [Matriks Coverage Repository](C2GRAPH-REPOSITORY-COVERAGE-MATRIX-ID.md) |
| Frozen shipped-path map | [File Map](C2GRAPH-FILE-MAP.md) | [File Map Indonesia](C2GRAPH-FILE-MAP-ID.md) |
| Notasi launcher Python | [CLI Launcher](C2GRAPH-CLI-LAUNCHER-GUIDE.md) | [CLI Launcher Indonesia](C2GRAPH-CLI-LAUNCHER-GUIDE-ID.md) |
| Diagnosis kegagalan | [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md) | [Troubleshooting Indonesia](C2GRAPH-TROUBLESHOOTING-ID.md) |

## Batas klaim coverage

Setelah overlay dokumentasi ini di-merge, target capability-level adalah **41/41 capability family pada README memiliki owner dokumentasi English dan Indonesia**. Hal ini berbeda dari klaim exact source-path coverage. Exact path coverage tetap harus dihasilkan dari authoritative release tree/manifest karena public File Map saat ini belum merekonsiliasi sendiri total runbook dan Python script yang dilaporkannya.
