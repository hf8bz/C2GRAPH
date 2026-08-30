# C2GRAPH Schema and Runtime State Catalog

**Bahasa Indonesia:** [C2GRAPH-SCHEMA-STATE-CATALOG-ID.md](C2GRAPH-SCHEMA-STATE-CATALOG-ID.md)

> C2GRAPH v1.2.0 public state map. This document distinguishes shipped schemas/templates, durable ledgers, generated runtime paths, and non-authoritative state.

## 1. Core rule

State can influence later work without automatically becoming repository-write authority. A checkpoint, cache hit, failure memory item, lesson, graph edge, or provenance record is evidence/state. Mutation still needs semantic scope + exact write plan + rollback + freshness + verification.

## 2. Goal state

- `.agent/goals.schema.md` — goal contract schema.
- `.agent/goals/` — emitted goal contracts/ledger location.

Goal state persists objectives across sessions/compactions. A Goal narrows intent; it is not arbitrary write permission.

## 3. Requirement state

- `.agent/requirements.schema.md` — requirement provenance/record schema.

Requirements distinguish received/derived implementation requirements from inference. They should retain their evidence/provenance relationship.

## 4. Work graph state

- `.agent/graph/schema/nodes.md`
- `.agent/graph/schema/edges.md`
- `.agent/graph/schema/example.jsonl`
- `.agent/graph/`

The graph represents engineering work relationships rather than requiring a graph database. Current public Reference vocabulary names 20 node types and 20 edge types. Schema validation must reject undeclared example types.

## 5. Failure and lesson state

- `.agent/failures.jsonl`
- `.agent/failures.schema.md`
- `.agent/lessons.jsonl`
- `.agent/lessons.schema.md`

Failure/lesson persistence is governed. Pre-authority observations may remain pending rather than being written simply to satisfy a bookkeeping desire. CEA can suppress actionability of exact recalled failure/lesson objects without rewriting their history or creating write authority.

## 6. Handoff and resume state

- `.agent/handoff.schema.md`
- `.agent/resume-capsule.schema.md`
- `.agent/state/`
- `.agent/runs.schema.md`
- `.agent/runs/`

These surfaces preserve continuation across compaction, budget exhaustion, CLI/session restart, and explicit resume. Current checkpoint rows are immutable; resume telemetry is a separate event rather than a rewrite of the checkpoint.

## 7. Provenance state

- `.agent/provenance.schema.md`
- `scripts/provenance/export.py`

The public Reference documents fourteen provenance fields and a SHA-256 digest chain. Digest integrity detects alteration after recording; it does not by itself authenticate who wrote a record.

## 8. Tool state

- `.agent/toolmap.schema.md`
- tool inventory/config surfaces

Tool state records capability and usage evidence. Discovery does not silently execute candidate tools when the contract says static discovery only, and tool availability does not prove semantic suitability.

## 9. Model-adapter state

- `.agent/model-adapter.schema.md`
- `adapters/c2modeladapter/**`

Provider/runtime/cache metadata remains at the adapter boundary. Cache telemetry and ACRP representation mode are non-authoritative with respect to repository writes.

## 10. Stable control-state surfaces

- `.agent/core-baseline.md`
- `.agent/rule-registry.md`
- `.agent/prefix.lock`
- `.agent/routes.yaml`
- `.agent/config.yml`
- `.agent/compliance-map.md`
- `.agent/distribution-files.txt`
- `.agent/counts.generated.json`
- `.agent/runtime-artifacts.txt`

These surfaces support drift detection, routing, configuration, compliance evidence mapping, package measurement, and generated-path declaration.

## 11. State lifecycle

A safe generic lifecycle is:

`observe -> classify -> bind evidence -> keep pending if no write authority -> materialize only when semantically required and authorized -> validate schema -> consume with freshness/containment rules -> supersede/close without erasing provenance`

Not every state family uses every step, but the direction is important: **observation does not imply persistence, and persistence does not imply authority**.

## 12. Generated versus shipped state

Empty `.gitkeep` locations may be shipped so runtime artifacts have known homes. Their existence does not mean populated runtime evidence exists in a pristine distribution. Verification must distinguish structural readiness from populated-runtime behavior.
