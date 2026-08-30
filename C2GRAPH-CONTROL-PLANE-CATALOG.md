# C2GRAPH Control Plane Catalog

**Bahasa Indonesia:** [C2GRAPH-CONTROL-PLANE-CATALOG-ID.md](C2GRAPH-CONTROL-PLANE-CATALOG-ID.md)

> C2GRAPH v1.2.0 public catalog. This document maps the repository-resident governance/control-plane families. It explains authority; it does not create it.

## 1. Authority order

A practical reading order is:

1. user/host objective and repository boundary;
2. selected entry mode and executable entry prompt;
3. `AGENTS.md` core invariants and routing;
4. `.agent/routes.yaml` + applicable scope(s);
5. applicable runbook(s);
6. `.agent/config.yml` thresholds and project binding;
7. exact Goal/requirement/evidence state;
8. PHASE 3.W exact `ACTUAL_WRITE_SET` + rollback basis + fresh pre-state;
9. immediate per-operation verification;
10. completion/evidence gates;
11. handoff/resume state where continuation is required.

No later explanatory document silently overrides an earlier executable authority contract.

## 2. Stable governance surfaces

| Surface | Role | Authority note |
| --- | --- | --- |
| `AGENTS.md` | root governance router and CORE invariants | normative control-plane surface |
| `.agent/routes.yaml` | machine-readable routing mirror | must remain consistent with Markdown routing |
| `.agent/core-baseline.md` | frozen shipped rule identifiers | detects silent rule loss |
| `.agent/rule-registry.md` | kept/moved/absorbed/rejected rule ledger | rejection requires explicit governance |
| `.agent/prefix.lock` | digest lock for stable router skeleton | detects unapproved structural drift |
| `.agent/config.yml` | centralized project bindings and thresholds | tunable values have one owner |
| `.agent/distribution-files.txt` | authoritative shipped-path manifest | count/parity basis |
| `.agent/counts.generated.json` | generated measurement snapshot | derived, not independent authority |
| `.agent/compliance-map.md` | evidence/control mapping | mapping, not certification |

## 3. Entry surfaces

| Entry surface | Formal mode | Intended use |
| --- | --- | --- |
| `SCAN-AND-ADOPT.prompt.md` | `ADOPT_WITH_GOAL` | existing repo adoption that proceeds into a goal-bound implementation path |
| `INSTALL-WORKFLOW-ONLY.prompt.md` | `WORKFLOW_ONLY` | install/repair/upgrade/verify only C2GRAPH control-plane surfaces in an existing repo |
| `BOOTSTRAP-NEW-REPO.prompt.md` | `BOOTSTRAP` | new/empty repository |

After installation, ordinary product work should use the normal goal-bound workflow rather than re-running an installation prompt for every session.

## 4. Domain scopes

The public File Map enumerates these scope surfaces:

- `_TEMPLATE.md`
- `backend-api.md`
- `data-schema.md`
- `frontend-web.md`
- `infra-deploy.md`
- `mobile-android.md`
- `mobile-ios.md`
- `mobile-native.md`
- `payments-sensitive.md`
- `qa-tooling.md`
- `ui-ux.md`

Scopes narrow/augment the root governance rules for a domain. A scope is not an independent permission source.

## 5. Runbooks

Runbooks encode operational procedures such as bounded reads, blocker handling, goal contracts, graph discovery, rollback, mobile release, model adapter admission, preflight recall, workflow discipline, and post-deploy QA.

Use `C2GRAPH-RUNBOOK-CATALOG.md` for the public runbook inventory and reconciliation status.

## 6. State and schema plane

Core state/schema families include:

- failures and failure schema;
- goals and goal schema;
- graph node/edge schemas + example;
- handoff schema;
- lessons + lesson schema;
- model-adapter schema;
- provenance schema;
- requirements schema;
- resume-capsule schema;
- runs schema;
- toolmap schema;
- runtime-artifact declarations;
- state/run/goal/graph directories.

Use `C2GRAPH-SCHEMA-STATE-CATALOG.md` for lifecycle details.

## 7. Evidence and provenance plane

C2GRAPH separates:

- raw repository evidence;
- graph/work relationships;
- requirements provenance;
- decision provenance;
- claims;
- compliance evidence mapping;
- verification output.

Provenance can be exported to W3C PROV-O JSON-LD. Digest chaining detects alteration of the recorded decision ledger; it is not identity authentication by itself.

## 8. Discovery plane

Discovery is bounded through repository scanners, corpus partitions, per-application index boundaries, graph discovery, secret-aware structural reads, Bounded Evidence Read, and non-authoritative discovery cache/recall.

Discovery can justify a finding. It does not itself authorize mutation.

## 9. Write/rollback plane

Repository mutation requires the canonical authority path:

- semantic scope;
- exact PHASE 3.W `ACTUAL_WRITE_SET`;
- rollback readiness appropriate to each path;
- fresh pre-state;
- exact action/target identity;
- immediate operation verification.

BEA v2 is a bounded human-granted emergency overlay for an already-materialized exact plan; it is not wildcard authority.

## 10. Representation, containment and liveness planes

ACRP, CEA, model/cache routing state, incident state, and progress-liveness diagnosis can alter representation, recall actionability, routing, or recovery strategy. They are non-interfering with the canonical exact-write predicate: they cannot manufacture new repository-write rows.

## 11. Handoff/resume plane

Handoff/resume preserves work state across context compaction, resource exhaustion, CLI/session restarts, or explicit continuation. Checkpoint state and resume telemetry remain evidence/state surfaces, not permission sources.

## 12. Verification plane

The public File Map reports 37 validators/gates and 39 registered checks for v1.2.0. The authoritative current count is produced by `scripts/verify/counts.py --json` inside the distribution.

Use `C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md` for verifier topology.

## 13. Non-authoritative explanatory surfaces

The root public guides, README, Deep Dive, File Map, catalogs, and diagrams explain and navigate the control plane. They must not silently change installer manifests, semantic scope, write authority, or measured source counts.
