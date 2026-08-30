# Katalog Runbook C2GRAPH

**English:** [C2GRAPH-RUNBOOK-CATALOG.md](C2GRAPH-RUNBOOK-CATALOG.md)

> Companion Bahasa Indonesia untuk `C2GRAPH-RUNBOOK-CATALOG.md`. **Status reconciliation:** public File Map melaporkan 40 runbook termasuk template (39 non-template), tetapi path table yang sama secara terlihat hanya menampilkan 36 Markdown runbook bernama. Dokumen ini tidak mengarang empat nama yang tidak terlihat.

## Runbook yang diekspos secara publik pada File Map

| Path | Tujuan ringkas |
|---|---|
| `.agent/runbooks/_TEMPLATE.md` | template runbook |
| `agent-dispatch.md` | bounded delegation |
| `batch-protocol.md` | batching |
| `blocker-handling.md` | klasifikasi/continuation blocker |
| `bounded-read.md` | bounded evidence read |
| `browser-tooling-recovery.md` | recovery browser/tool integration |
| `budget-resume.md` | resource/context suspension + resume |
| `context-compaction.md` | continuity setelah compaction |
| `coverage-backfill.md` | completeness census/backfill |
| `db-migration.md` | schema/data migration |
| `discovery-cache.md` | cross-session discovery cache + invalidation |
| `edit-tooling.md` | mechanical multi-file editing |
| `entity-resolution.md` | canonical entity resolution |
| `evidence-contract.md` | contract untuk claim/finding/verification evidence |
| `goal-contract.md` | Goal contract |
| `goal-inheritance.md` | goal inventory/lineage |
| `graph-discovery.md` | bounded repository/graph discovery |
| `incident-rollback.md` | production incident rollback |
| `mobile-build-release.md` | mobile build/release |
| `mobile-device-qa.md` | mobile device QA |
| `mobile-store-submission.md` | store submission |
| `model-adapter.md` | adapter/runtime/cache diagnosis |
| `monorepo-index-placement.md` | per-application index placement |
| `plan-to-docs.md` | durable plan documentation |
| `post-deploy-qa.md` | post-deploy verification |
| `preflight-recall.md` | pre-action failure recall |
| `propagation.md` | propagasi koreksi manusia |
| `ratchet-loop.md` | measurable ratchet loop |
| `rule-preservation.md` | preservasi rule saat merge/router change |
| `simplification-ladder.md` | cek sebelum menambah abstraction/dependency |
| `system-inventory.md` | inventory tool/system untuk claim limits |
| `test-to-workflow.md` | mapping test ke workflow/claim |
| `tool-usage-capture.md` | capture tool usage dari real call sites |
| `ui-audit.md` | UI/visual QA governance |
| `untrusted-content.md` | handling untrusted content |
| `workflow-discipline.md` | workflow/rulebook discipline |

## Reconciliation rule

Jangan membuat nama runbook ke-37 sampai ke-40 berdasarkan tebakan. Generate exact catalog dari authoritative source/release tree dan validasi terhadap output `scripts/verify/counts.py --json`.
