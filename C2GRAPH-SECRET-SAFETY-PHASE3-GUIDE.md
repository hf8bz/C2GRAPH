# C2GRAPH v1.1.0 - CORE-33 Secret Safety and PHASE 3 Guide
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

> Canonical operator guide for secret-safe discovery and the single human stop boundary used by
> CORE-33. This guide describes the contract shipped in the `v1.1.0` Version 1 release, dated **13 August 2026**; it is not a later-version change.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) - [Safe GATE 0](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) - [Reference](C2GRAPH-REFERENCE.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md)

## 1. The rule in one sentence

**A secret value discovered before PHASE 3 is contained immediately but does not globally stop the run
at PHASE 0, 1, or 2; it becomes a mandatory human blocker at PHASE 3.**

This routing applies to CORE-33 secret exposure. It does not disable unrelated STOP rules such as an
unsafe write path, destructive action without authority, or a prompt-injection decision that independently
requires human review.

## 2. Why PHASE 1.13 must not be the global stop

PHASE 0-2 are evidence-gathering phases. A secret exposure there can be contained without making a
product or workflow mutation. Stopping the whole run at the discovery line creates three problems:

1. the human-decision boundary becomes scattered across discovery instead of concentrated at PHASE 3;
2. independent safe evidence can no longer be collected, so the human receives a poorer decision packet;
3. an accidental read becomes a denial-of-service primitive even after the unsafe slice is quarantined.

The correct response is therefore **contain now, decide at PHASE 3**.

## 3. Prevention comes before incident routing

The preferred path is that raw secret values never reach model context at all. Files likely to contain
credentials must not be direct-read during discovery. Examples include:

- `.env`, `.env.*`, `.dev.vars`, and `.dev.vars.*`;
- `wrangler.toml`, `wrangler.*.toml`, and `wrangler.local-dev.toml`;
- `.npmrc`, `.pypirc`, and `.netrc`;
- filenames containing `secret`, `credential`, `password`, `api-key`, `access-key`,
  `private-key`, or `auth-token`;
- service-account JSON, SSH private-key names, and `.pem`/`.key`/`.p12`/`.pfx` containers;
- any other configuration already suspected of holding credential material.

Use the shipped structural reader instead:

```bash
<C2PY> scripts/scan/redact.py --target <TARGET_ROOT> --path wrangler.local-dev.toml
```

The reader emits structural evidence such as path, line number, key class/fingerprint,
section fingerprint, and redaction kind. It does **not** emit raw assignment values, raw keys,
raw section names, comments, or raw source-line text. It uses Python standard-library operations only, launches no subprocess,
makes no network request, writes no target byte, rejects symlink paths and target escape, and refuses
unsafe/non-regular inputs.

A raw `cat`, `sed`, `head`, editor dump, unrestricted `directRead`, shell interpolation, or generic file
reader is not an acceptable substitute for secret-prone files.

## 4. Canonical state machine

### 4.1 No secret exposure

Continue the normal entry-mode state machine.

### 4.2 Suspected secret-prone file before raw content is loaded

1. do not direct-read it;
2. route it through `scripts/scan/redact.py`;
3. keep only structural/name/location evidence;
4. continue discovery normally.

No incident exists merely because a sensitive file exists.

### 4.3 A secret value accidentally reaches context in PHASE 0, 1, or 2

Perform containment immediately:

1. **do not repeat the value** in a response, ledger, graph, handoff, error message, command, or incident file;
2. quarantine the affected source/slice from further raw reads;
3. switch any necessary structural inspection to `scripts/scan/redact.py`;
4. stage a sanitized `PENDING_SECURITY_INCIDENT` in session/host evidence;
5. record only safe metadata: incident id, source path, location if known, secret class/key fingerprint if safely
   known, phase, exposure channel, and mitigation status;
6. continue only discovery slices that are independent, already proven safe, read-only, and do not require
   the exposed value or the quarantined raw source;
7. mark unavailable evidence `unknown` rather than reopening the unsafe slice;
8. carry the pending incident to PHASE 3.

**Do not emit `RUN_OUTCOME: stopped` or `STOP_AT: PHASE 1.13 / CORE-33` for this condition.** The run is
still progressing toward its mandatory human decision boundary.

### 4.4 PHASE 3

Every `PENDING_SECURITY_INCIDENT` becomes a mandatory CORE-33 blocker. There is no default and no
autonomous answer. The human must choose an evidence-backed resolution. Canonical outcomes are:

- **mitigated** - the credential was rotated/revoked or otherwise invalidated as appropriate, unsafe
  discovery was corrected, and continuation is approved;
- **false positive / not a credential** - the human confirms the observed material did not constitute a
  secret, with enough rationale to make that decision auditable;
- **mitigation incomplete** - stop the run **at PHASE 3**. Do not enter a write-bearing later phase.

A PHASE 3 resolution does not itself authorize persistence. A sanitized incident artifact may be
persisted only when its exact post-3.W destination/action is semantically required, appears in
`ACTUAL_WRITE_SET`, has path-scoped authority, and is fresh. The leaked value itself must never be
persisted by C2GRAPH.

### 4.5 Exposure after PHASE 3

Contain immediately, then return to the PHASE 3 decision barrier before any further action. Do not keep
executing PHASE 4-8 simply because an earlier PHASE 3 was already completed.

## 5. Interaction with rollback authority

CORE-33 routing and rollback authority are separate dimensions. The rollback side is defined in [C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md):

- before `ROLLBACK_AUTHORITY` is proven, `PENDING_SECURITY_INCIDENT` lives outside target bytes;
- proving rollback authority does not resolve the security incident;
- resolving the security incident does not create rollback authority;
- a write after PHASE 3 still requires the normal preflight/fingerprint and write-scope rules.

A secret exposure therefore cannot be used to justify an early `.agent/incidents/**` write.

## 6. What may continue before PHASE 3

Allowed after containment:

- static file/path census that does not output sensitive values;
- secret-safe structural scans through `scripts/scan/redact.py`;
- unrelated source/document inventory already known not to contain the exposed value;
- pending-evidence construction in session/host memory;
- deterministic calculations over already-sanitized evidence.

Not allowed:

- rereading the offending raw line to "confirm" the value;
- searching for the literal leaked value across the repository;
- passing the value into another scanner, model, shell command, URL, or log;
- making product/workflow writes before the normal authority boundary;
- pretending the incident is resolved because discovery continued.

## 7. Reporting contract

Before PHASE 3, report the incident only as an in-progress sanitized state, for example:

```text
SECURITY_INCIDENT      : pending_phase3
SECURITY_INCIDENT_ID   : SEC-001
SECURITY_SOURCE        : wrangler.local-dev.toml:<redacted-span>
SECRET_VALUE_REPEATED  : no
AFFECTED_SLICE         : quarantined
SAFE_DISCOVERY         : continuing_independent_slices
NEXT_BARRIER           : PHASE 3 / CORE-33
```

Do not report the value. Do not claim `TARGET_MUTATIONS: 0` unless mutation coverage genuinely proves it;
use `unknown` when coverage is incomplete.

If mitigation remains incomplete at PHASE 3, the stopped report should name PHASE 3, not the earlier
discovery location:

```text
RUN_OUTCOME       : stopped
STOP_AT           : PHASE 3 / CORE-33 security decision
SECURITY_INCIDENT : mitigation_incomplete
NEXT_SAFE_ACTION  : rotate/revoke/mitigate and rerun or resume from the PHASE 3 barrier
```

## 8. WORKFLOW_ONLY behavior

For `ENTRY_MODE=WORKFLOW_ONLY`, secret handling does not create a goal and does not relax the no-product
mutation boundary. Goal state is preserved exactly as required by the workflow-only contract.

A secret exposure in an existing repo with no goal therefore follows this order:

```text
WORKFLOW_ONLY
  -> PHASE 0-2 read-only discovery
  -> secret exposure, if any
  -> contain + quarantine + PENDING_SECURITY_INCIDENT
  -> continue independent safe read-only discovery
  -> PHASE 3 mandatory human CORE-33 decision
  -> if resolved, continue normal workflow-only installation
  -> PHASE 8 WORKFLOW_ONLY branch
  -> final report; no goal mutation
```

## 9. ADOPT_WITH_GOAL and BOOTSTRAP behavior

The same CORE-33 barrier applies to the other entry modes. The difference occurs after the incident is
resolved:

- `ADOPT_WITH_GOAL` continues the adoption path and may later emit the goal according to its normal gate;
- `BOOTSTRAP` continues rulebook/bootstrap construction and later goal emission according to its own gate;
- neither mode may bypass PHASE 3 because the secret was encountered earlier.

## 10. Failure combinations

Multiple pending failures do not change the security routing. For example, missing optional tooling,
regex no-match, test-count uncertainty, and one secret exposure may coexist. Non-security failures remain
classified under their own rules. The secret incident is still carried to PHASE 3 and must be resolved
there.

The presence of four pending tooling/discovery failures is **not** a reason to stop at the secret's PHASE
1 location unless one of those other failures independently satisfies a different STOP rule.

## 11. Acceptance tests

The shipped self-test includes regression fixtures that must stay green:

1. a `wrangler.local-dev.toml` fixture containing value, key, section, comment, and opaque-line
   sentinels is scanned with `redact.py`;
2. none of those raw sentinels appears in stdout;
3. key class/fingerprint, section fingerprint, line, and redaction-kind metadata do appear;
4. target digest is unchanged;
5. a symlink to an outside credential file is refused without exposing its content;
6. core contracts contain `PENDING_SECURITY_INCIDENT` + PHASE 3 routing;
7. stale immediate-secret-stop language and the old absolute context guarantee are rejected by the
   contract test.

The purpose of these tests is to make the incident that motivated this rule a non-regressing behavior,
not merely a documentation convention.

## 12. Operator checklist

Before approving a rerun or PHASE 3 continuation, confirm all of the following:

- the leaked value has not been repeated in the report;
- the affected raw source is quarantined;
- secret-prone reads now use the redacting reader or are skipped;
- the incident is present as sanitized `PENDING_SECURITY_INCIDENT` evidence;
- any real credential has been rotated/revoked/invalidated as appropriate;
- PHASE 3 contains the explicit human decision;
- rollback/write authority is independently valid before any target write;
- no goal/product mutation occurred merely because CORE-33 fired;
- the final stopped phase, if mitigation is incomplete, is PHASE 3 rather than PHASE 0-2.
