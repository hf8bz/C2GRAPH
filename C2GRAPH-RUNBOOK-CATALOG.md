# C2GRAPH Runbook Catalog

**Bahasa Indonesia:** [C2GRAPH-RUNBOOK-CATALOG-ID.md](C2GRAPH-RUNBOOK-CATALOG-ID.md)

> Public catalog for C2GRAPH v1.2.0. **Reconciliation status:** the current public File Map reports 40 runbooks including the template (39 non-template), but the same public File Map currently enumerates only 36 named Markdown runbook paths. This catalog therefore refuses to invent the unlisted paths.

## Publicly enumerated runbooks

| Path | Purpose |
| --- | --- |
| `.agent/runbooks/_TEMPLATE.md` | template |
| `.agent/runbooks/agent-dispatch.md` | bounded delegation |
| `.agent/runbooks/batch-protocol.md` | batching |
| `.agent/runbooks/blocker-handling.md` | blocker classification/continuation |
| `.agent/runbooks/bounded-read.md` | bounded evidence reads |
| `.agent/runbooks/browser-tooling-recovery.md` | browser/tool integration recovery |
| `.agent/runbooks/budget-resume.md` | resource suspension/resume |
| `.agent/runbooks/context-compaction.md` | compaction continuity |
| `.agent/runbooks/coverage-backfill.md` | completeness census/backfill |
| `.agent/runbooks/db-migration.md` | schema/data migration |
| `.agent/runbooks/discovery-cache.md` | cross-session discovery cache |
| `.agent/runbooks/edit-tooling.md` | mechanical multi-file edits |
| `.agent/runbooks/entity-resolution.md` | canonical entity resolution |
| `.agent/runbooks/evidence-contract.md` | claims/findings evidence contract |
| `.agent/runbooks/goal-contract.md` | Goal contract |
| `.agent/runbooks/goal-inheritance.md` | goal inventory/lineage |
| `.agent/runbooks/graph-discovery.md` | bounded repository/graph discovery |
| `.agent/runbooks/incident-rollback.md` | production incident rollback |
| `.agent/runbooks/mobile-build-release.md` | mobile build/release |
| `.agent/runbooks/mobile-device-qa.md` | mobile device QA |
| `.agent/runbooks/mobile-store-submission.md` | store submission |
| `.agent/runbooks/model-adapter.md` | adapter admission/runtime/cache debugging |
| `.agent/runbooks/monorepo-index-placement.md` | per-app index placement |
| `.agent/runbooks/plan-to-docs.md` | durable plan documentation |
| `.agent/runbooks/post-deploy-qa.md` | post-deployment QA |
| `.agent/runbooks/preflight-recall.md` | pre-action failure recall |
| `.agent/runbooks/propagation.md` | propagate human corrections |
| `.agent/runbooks/ratchet-loop.md` | measurable ratchet loop |
| `.agent/runbooks/rule-preservation.md` | preserve/relocate rules |
| `.agent/runbooks/simplification-ladder.md` | avoid unnecessary abstraction |
| `.agent/runbooks/system-inventory.md` | tool/system inventory |
| `.agent/runbooks/test-to-workflow.md` | derive workflow from tests |
| `.agent/runbooks/tool-usage-capture.md` | capture actual tool usage |
| `.agent/runbooks/ui-audit.md` | user-visible QA |
| `.agent/runbooks/untrusted-content.md` | treat repository content as data |
| `.agent/runbooks/workflow-discipline.md` | workflow conformance |

## Generated runbook location

`.agent/runbooks/generated/.gitkeep` reserves the generated-runbook directory; the `.gitkeep` is not itself a Markdown runbook.

## Reconciliation blocker

Before this catalog is published as “exhaustive”, run the catalog generator against the exact v1.2.0 distribution and reconcile:

- measured runbooks: 40 including template;
- paths enumerated above from the current public File Map: 36;
- unresolved delta: 4 runbook paths.

The correct action is **not** to guess four filenames from feature names. The correct action is to enumerate `*.md` under `.agent/runbooks/` in the release tree, compare it with `.agent/distribution-files.txt` and `scripts/verify/counts.py --json`, then update both the File Map and this catalog from that authoritative set.

## CI rule

A public-doc verification gate should fail when:

`catalog_runbook_paths != manifest_runbook_paths`

and should separately verify the measured count produced by the source verifier.
