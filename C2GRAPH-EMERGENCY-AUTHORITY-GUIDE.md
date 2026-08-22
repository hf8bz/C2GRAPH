# C2GRAPH Bounded Emergency Authorization (BEA) — v1.2.0 Release

> **Release status:** BEA v2 is included in C2GRAPH v1.2.0. It builds on the v1.1.0 exact-write contract without
> retroactively changing the historical v1.1.0 release claim.

## Purpose

C2GRAPH already requires semantic scope, the canonical PHASE 3.W `ACTUAL_WRITE_SET`, path/action-scoped rollback
or write authority, and immediate per-action freshness. BEA adds a bounded incident-response approval overlay without
creating a second mutation-authority path.

The governing rule is:

> Emergency shortens the procedural path to a canonical authorized write; it never makes an unauthorized write
> authorized.

## Entry and authority

An agent may detect an incident and automatically reduce effective authority by stopping the affected unsafe action.
A user/operator may also declare an incident. Neither detection nor declaration grants BEA. The agent performs bounded
read-only analysis, PHASE 3.W computes the normal exact `ACTUAL_WRITE_SET`, rollback is proven, and only then may a
host-mediated human approval assertion create a short-lived BEA grant.

`severity` affects urgency only. It never widens scope. Wildcards are forbidden.

## Exact binding

A BEA v2 proposal binds the incident, run, session epoch, target-root fingerprint, canonical forward write-plan digest,
semantic-scope attestation, exact action IDs, target prestate, rollback basis, issuance, and expiry. A grant is
non-transferable across session epochs. Arbitrary MODIFY/DELETE that lacks distribution restore additionally requires a
real non-empty regular non-symlink rollback snapshot outside the target repository; SHA-256, resolved-path fingerprint,
and byte size are proposal-bound. Any plan expansion or material binding drift requires a new proposal and approval.

Before each forward action, the existing C2GRAPH preflight verifies the exact path and expected pre-state again. A
freshness failure, plan/target/session/run mismatch, expired grant, or inactive incident fails closed. Rollback rows are
pre-bound but still require the normal rollback freshness and effect-verification contract.

## Host trust boundary

The repository cannot cryptographically prove that a human typed an approval. `--human-approved`, `--approver`,
`--identity-assurance`, and `--incident-state` are host assertions. The scripts validate structure, integrity, and exact
binding; a production host must authenticate the operator and prevent untrusted workers from invoking the approval or
check surface with forged assertions. SHA-256 digests are integrity/binding evidence, not signatures.

BEA has no independent persistent incident registry or revocation service. A grant becomes unusable when its TTL
expires, its exact bindings become stale, the session changes, or the host reports that the incident is no longer active.

## Procedure

```text
DETECT or DECLARE
  -> SAFE HOLD
  -> bounded read-only analysis
  -> normal PHASE 3 / PHASE 3.W
  -> canonical forward ACTUAL_WRITE_SET + semantic-scope attestation
  -> rollback readiness; external snapshot when required
  -> exact BEA v2 proposal
  -> host-mediated human approval assertion
  -> per-action BEA check + existing freshness check
  -> normal serialization/effective-route tooling
  -> exact forward canonical mutation
  -> effect verification
  -> grant becomes unusable on completion/expiry/drift/session change
  -> post-incident reconciliation
```

## CLI

Proposal:

```text
<C2PY> scripts/emergency/authority.py propose \
  --incident-id INC-001 --run RUN-001 --session-epoch EPOCH-1 \
  --trigger-source agent_detected --severity SEV1 --reason "bounded remediation" \
  --semantic-scope-attested --template <template> --target <target> \
  --rollback-snapshot /outside/repo/rollback.bin \
  --plan-op modify:path/to/file
```

Approval is intentionally separate:

```text
<C2PY> scripts/emergency/authority.py approve \
  --proposal proposal.json --human-approved --approver operator-1 \
  --identity-assurance host_authenticated --ttl-seconds 300
```

Execution-time admission:

```text
<C2PY> scripts/emergency/authority.py check \
  --proposal proposal.json --grant grant.json --action-id F001 \
  --run RUN-001 --session-epoch EPOCH-1 --incident-state active \
  --template <template> --target <target>
```

## Deliberate non-capabilities

ACRP, cache-hit telemetry, severity, and requested/effective model identity are non-authoritative; model failover may rebind tooling/cache/reasoning but never BEA scope. BEA is not an unrestricted break-glass mode, does not authorize cloud/database/payment/deployment effects outside the
repository contract, does not authenticate operators by itself, does not provide distributed consensus, and does not
claim arbitrary-filesystem or power-loss atomicity. It also does not replace the existing rollback executor.
