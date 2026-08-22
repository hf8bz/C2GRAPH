# C2GRAPH Cognitive Emergency Authorization (CEA)

**Release status:** included in C2GRAPH v1.2.0. C2GRAPH CEA is intentionally narrower than C2Cognitive CEA.

## Why C2GRAPH needs a narrow CEA

C2GRAPH has durable failure-memory and lesson ledgers that can affect later recall. During an incident, an exact
poisoned
`FAIL-*` or `LSN-*` record may need to stop being actionable immediately. Those ledgers are repository bytes, so letting
CEA rewrite them directly would create a second repository-write authority beside BEA.

C2GRAPH therefore uses an external containment overlay:

```text
incident evidence
  -> exact failure/lesson IDs + canonical record digests
  -> explicit human CEA approval
  -> short-lived non-transferable grant
  -> external suppression overlay
  -> recall selector returns raw evidence but actionable=false
```

## Authority separation

CEA does not authorize repository/product writes, inherit BEA, promote trust, widen ACL, override freshness, or gain
power from ACRP/cache/model routing. Durable correction/deletion of a ledger still requires normal write authority or
BEA. If poisoned content was already consumed by the model, a host reset/new session is required before containment is
considered effective.

Wildcards and class/signature-wide suppression are forbidden. Each object is bound to its current record digest; record
mutation invalidates the grant. Grants are run/session/incident bound, expire in at most 900 seconds, and stop working
when the incident is resolved/revoked.

## Verification

```text
<C2PY> scripts/verify/cognitive_emergency.py
<C2PY> scripts/selftest/cognitive_emergency.py
<C2PY> scripts/selftest/fullstack_interactions.py
```

The current candidate verifies raw-evidence retention, exact-ID/digest binding, cross-session and resolved-incident
refusal, wildcard refusal, external-only output, consumed-content reset, and CEA/BEA/ACRP/cache/model separation.
## 19 August CLI hardening

Function-surface audit found and closed `C2G-CEA-20260819-02`: `main()` reused the imported `c2common` alias for
the `check` subparser and could fail before command handling. The local parser variable was renamed, and the CEA
selftest now executes the real CLI entry path as a regression case. Current directed evidence is **18/18 per edition**.
This was an availability/correctness defect; no authority-widening path was observed because execution failed before
authorization.

