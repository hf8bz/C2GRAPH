# C2GRAPH Graph and Work-State Lifecycle Guide

**Bahasa Indonesia:** [C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE-ID.md](C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE-ID.md)

> C2GRAPH v1.2.0 guide to the state of engineering work. This is the C2GRAPH-native analogue of a cross-cutting state guide; it does not introduce C2Cognitive persistent-cognition features.

## 1. State categories

C2GRAPH makes selected engineering state explicit:

- objective and Goal;
- requirements;
- plan/tasks;
- evidence and sources;
- graph relationships;
- decisions/provenance;
- exact write plan and rollback basis;
- runs and verification;
- failures and lessons;
- checkpoints/handoff/resume;
- tool/model/runtime capability evidence.

## 2. Lifecycle overview

```text
Objective
  -> Goal
  -> Requirements + Evidence
  -> Plan / Task graph
  -> exact write proposal
  -> rollback + fresh pre-state
  -> authorized mutation
  -> verification
  -> Artifact / Claim / Evaluation
  -> Failure or Lesson when applicable
  -> Checkpoint / Handoff / Resume when continuation is needed
```

Edges between these concepts should remain inspectable. A later stage does not erase the evidence or authorization requirements of an earlier stage.

## 3. Goal lifecycle

A Goal persists an objective across turns/compactions. Goal state can be absent, empty, active, terminal-only, inactive, or invalid in the workflow-only decision logic. Installation does not fabricate an active product goal.

## 4. Requirement lifecycle

Requirements should be traceable to received evidence or a clearly marked derivation. An inference should not silently become a product requirement. Contradictions must remain visible until resolved by appropriate evidence/human decision.

## 5. Evidence lifecycle

Evidence is discovered/read -> bounded -> attributed -> classified -> used to support/contradict claims -> revalidated when freshness matters. Evidence does not automatically become write authority.

## 6. Plan/write lifecycle

A semantic plan becomes mutation-authorizing only after the exact write set and rollback/freshness conditions are materialized. If a later phase discovers a new write path, return to the write-plan authority phase; never silently expand the set.

## 7. Failure and lesson lifecycle

Failures and lessons may be recalled before repeating a known-bad approach. Their persistence and actionability remain governed. Stale/poisoned recalled items can be contained; containment does not rewrite history.

## 8. Checkpoint/resume lifecycle

Checkpoint state exists to preserve continuity, not permission. Resume reconstructs bounded work state and continues from verifiable evidence. If no durable checkpoint/capsule survives, reconstruct from evidence rather than inventing completed work.

## 9. Model/cache state

Provider capability, routing, cache state, and ACRP representation are runtime/strategy inputs. They must not leak into the repository authority plane.

## 10. Closure

A work item is complete only to the extent that its completion claim is supported by the required verification evidence. “Agent says done” is not a lifecycle state with independent authority.
