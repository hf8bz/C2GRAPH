# C2GRAPH Adaptive Context Representation Planning (ACRP)

**Release status:** included in C2GRAPH v1.2.0. This capability was not part of the historical v1.1.0 release bytes.

## Purpose

ACRP optimizes how a semantic set that C2GRAPH has already bounded and frozen is presented to the effective model.
It never decides what evidence enters the set, never expands authority, and never turns cache/model telemetry into
correctness evidence.

Canonical order:

```text
bounded retrieval + trust/freshness/evidence selection
  -> freeze ordered semantic set and selection digest
  -> ACRP
  -> effective-route model adapter
  -> LLM
```

## Runtime modes

- `native`: no representation transform; always valid.
- `canonical`: deterministic canonical representation without semantic expansion.
- `segmented`: at most one pre-admitted codec for each eligible segment.
- `aggressive`: disabled in v1.

Authority, evidence, rollback, source code, security policy, BEA artifacts, and CEA suppression artifacts stay native.
A segmented profile must be fresh, route-matched, bound to the stable adapter representation contract, and backed by
external live-model semantic/productivity evidence. Missing/stale/mismatched evidence falls back to native/canonical.

## Failover

A physical alias such as a bounded Luna suffix may reuse the semantic representation profile when the stable canonical
model/runtime contract is unchanged, while physical provider cache identity remains isolated. Cross-family failover
invalidates the old representation profile. If no admitted profile exists for the new effective family, ACRP uses the
native safe path.

## Safety boundary

ACRP accepts only `c2graph_context_selection_v1` with `selection_frozen=true`. It cannot add records after compression,
change record order, widen ACL/trust/freshness, authorize writes, or satisfy completion. Materialized payload/plan files
must be new absolute paths outside the repository.

## Verification

Run:

```text
<C2PY> scripts/verify/context_representation.py
<C2PY> scripts/selftest/context_representation.py
<C2PY> scripts/selftest/fullstack_interactions.py
```

The current candidate selftest includes exact round-trips, route/profile mismatch, cross-family failover, tamper and
truncation, missing-vs-null preservation, output-path safety, and 1,000 randomized exact/order/determinism cases.
