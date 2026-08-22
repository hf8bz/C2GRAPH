# C2GRAPH Blocker Convergence Guide -- v1.2.0
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

This guide defines when a C2GRAPH run must continue, when it may narrow claims and continue, and when
it must actually stop. Its purpose is to prevent circular or persistence-only conditions from being
misclassified as blockers while retaining real safety barriers.

Use this guide with `C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md`. The executable source of truth is
still the selected entry prompt plus `scripts/install/preflight.py` and the shipped runbooks.

## 1. Core rule: a blocker must prevent the next required action

A condition is a hard blocker only when all three are true:

1. the next action is required by the selected entry mode;
2. that action cannot safely proceed on an independent branch or with a proven fallback; and
3. the missing decision, capability, scope, or rollback evidence cannot be deferred without making the
   required action unsafe.

Pending observability is not a blocker by itself. `PENDING_FAILURE`, `PENDING_DECISION`,
`PENDING_DISCOVERY_CACHE`, `PENDING_DISCIPLINE`, `PENDING_HANDOFF`, and claim limits do not count
against `blocker_stop_threshold` unless the underlying unresolved condition prevents a required next
action.

## 2. Convergence order

Every run converges blockers in this order:

```text
PHASE 0 preliminary evidence
  -> PHASE 0-2 target-byte-read-only discovery
  -> PHASE 3 human decisions / CORE-33 resolution
  -> PHASE 3.W final desired bytes
  -> remove byte-identical no-ops
  -> semantic-scope validation
  -> ACTUAL_WRITE_SET create|modify|delete
  -> plan-specific rollback classification
  -> optional named rollback-unit overlay for exact snapshot-needed paths
  -> transaction-start freshness
  -> derived MKDIR freshness + explicit top-down creation
  -> per-file target/source/named-unit freshness
  -> mutation
  -> verification gates
```

A later concern must not be moved earlier merely to make bookkeeping convenient.

## 3. Conditions that MUST NOT stop the run by themselves

### 3.1 Preliminary collision counts

`new files`, `identical collisions`, and `DIFFERING collisions` are PHASE 0 inventory. They are not
final authority and they are not blockers until the final write plan needs one of those paths.

The historical pattern:

```text
new files             0
identical collisions  745
DIFFERING collisions  0
rollback_candidate    new_files_only
```

must not be interpreted as rollback permission. If those 745 paths are outside the final byte-changing
write set, they become irrelevant to rollback classification.

### 3.2 `_decisions.md` cannot yet be written

Human answers are captured as ordered `PENDING_DECISION` session evidence. The next PHASE 3 question
continues immediately. `_decisions.md` persistence is optional unless that exact persistence is itself
semantically required and deliberately included in `ACTUAL_WRITE_SET` after PHASE 3.W.

Therefore this is invalid:

```text
STOP_AT: PHASE 3 -- existing-file decision persistence blocked
```

when the only unavailable action is writing the audit row.

### 3.3 Pending failure or cache persistence

A failure can remain `PENDING_FAILURE`; discovery cache, discipline, handoff, and tooling state can stay
in session evidence. These records never widen semantic scope and never become automatic writes just
because some other path has rollback authority.

### 3.4 Optional or unrelated tooling failure

A missing optional tool, unverified package manager, or failed capability unrelated to the current
required action narrows claims. It does not globally stop PHASE 4. Only a capability actually required
for the next action and lacking an approved safe fallback becomes a hard blocker.

### 3.5 Byte-identical desired result

Before `ACTUAL_WRITE_SET` exists, compute the final desired bytes. If they are already identical to the
target, the operation is a no-op and must be omitted. A no-op cannot become a fake MODIFY merely to
persist an audit event or make installation look active.

## 4. Legitimate PHASE 3.W blockers

### 4.1 Semantic scope violation

Rollback safety does not grant scope. `WORKFLOW_ONLY` may never modify product/source behavior even if
the path is a safe CREATE with easy rollback. Re-plan or stop that branch.

### 4.2 Unsafe or ambiguous filesystem path

Symlink/junction/link-like redirection, mount-point parent, hardlinked existing target, file/directory
shape conflict, ancestor/descendant regular-file plan conflict, non-portable Windows path grammar,
NFC/case alias, or a destination whose meaning is host-dependent is fail-closed.

### 4.3 Existing customized planned write lacks a before-image

If a real planned MODIFY/DELETE targets an existing customized file and the independent C2GRAPH source
does not contain the current before-image, automatic preflight reports:

```text
rollback_candidate snapshot_or_vcs_required
```

This is a legitimate `permission` blocker until a named rollback unit exists.

## 5. Named snapshot/VCS continuation -- no retry loop

`snapshot_or_vcs_required` is a diagnostic from automatic rollback classification. It is not expected
to disappear merely because the human supplied an external rollback unit.

Ask one `Q-ROLLBACK` for the printed `? SNAPSHOT` paths. On approval record:

```text
ROLLBACK_UNIT          : <stable snapshot/archive/VCS name or revision>
RESTORE_OWNER          : <person/system responsible for restore>
COVERED_PATHS          : <exact printed snapshot-needed paths>
ROLLBACK_UNIT_EVIDENCE : <archive SHA-256, revision id, or equivalent immutable/read-only proof>
WRITE_PLAN_SHA256      : <plan fingerprint>
ROLLBACK_AUTHORITY     : proven:<named-unit>
```

Authority is path-scoped. It does not cover unrelated paths and does not grant semantic scope.

Do **not** re-run automatic preflight expecting `snapshot_or_vcs_required` to turn into
`distribution_restore`. Keep its target `EXPECT_SHA256` rows and overlay the named rollback authority
for only the covered paths. Before the first and every dependent MODIFY/DELETE, confirm the named unit
is still accessible and unchanged using its unit-specific read-only evidence method. If that cannot be
proven, the blocker remains legitimate.

## 6. CREATE and derived parent directories are separate actions

A CREATE plan may imply missing parent directories. Preflight emits each missing parent as:

```text
+ MKDIR <relative-directory>
> EXPECT_DIR_ABSENT <relative-directory>
```

Process them top-down. Before each directory creation run:

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" --target "$TARGET_ROOT" \
  --verify-op mkdir:<relative-directory> --expect-state absent
```

Only `freshness_verified yes` permits creation of that one directory. Do not use recursive parent
creation. Before the file CREATE, run its own `--verify-op create:<path> --expect-state absent`.
CREATE must refuse while any implied parent remains absent.

This prevents an actor from creating a parent after PHASE 3.W and having C2GRAPH mistakenly claim that
parent as its rollback unit.

Rollback order is the inverse: delete exact files, then remove only directories created by this run,
deepest-first and only while empty.

## 7. Existing shipped restore source must also remain fresh

For `distribution_restore`, plan output includes:

```text
EXPECT_SHA256 <target-preimage> <path>
EXPECT_SOURCE_SHA256 <source-preimage> <path>
```

Immediately before that MODIFY/DELETE:

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" --target "$TARGET_ROOT" \
  --verify-op modify:<path> --expect-state sha256:<TARGET_HASH> \
  --expect-source-state sha256:<SOURCE_HASH>
```

Both target and independent restore source must still be fresh. Source drift returns the path to
PHASE 3.W before write.

## 8. CORE-33 remains a PHASE 3 human barrier

A secret exposure discovered in PHASE 0-2 is contained immediately, not repeated, quarantined, and
staged as `PENDING_SECURITY_INCIDENT`. Safe independent read-only discovery may continue. The global
human stop for that incident occurs at PHASE 3/Q-SEC. If mitigation is incomplete there, stop. Do not
move that stop back into PHASE 1 merely because the exposure was discovered there.

## 9. Blocker classification table

| Condition | Stop now? | Correct treatment |
| --- | --- | --- |
| preliminary 745 identical collisions | no | inventory only; wait for actual write set |
| `_decisions.md` cannot be written | no | keep `PENDING_DECISION` |
| 6 pending failures, none blocking next action | no | report pending; threshold does not count them |
| optional tool missing | no | claim limit / fallback |
| required tool missing, no safe fallback | yes at PHASE 4 | `tool_absent` or relevant gate blocker |
| CORE-33 exposure in PHASE 1 | no global stop there | contain; carry to PHASE 3 |
| CORE-33 mitigation incomplete at PHASE 3 | yes | human security barrier |
| no-op existing file | no write | omit from write set |
| CREATE parent missing | not a human blocker | verify explicit MKDIR top-down |
| derived parent appears externally | yes for that path | freshness red; return PHASE 3.W |
| pristine shipped existing MODIFY | normally no | `distribution_restore` if source stays fresh |
| customized planned existing MODIFY, no before-image | yes until named unit | `permission`, Q-ROLLBACK |
| named unit supplied and coverage proven | no | bind path-scoped authority and continue |
| named rollback unit later changes/disappears | yes for dependent write | return PHASE 3.W |
| product path in WORKFLOW_ONLY | yes for that branch | scope violation; remove/re-plan |
| new unplanned write discovered later | yes before that write | return PHASE 3.W and re-authorize |

## 10. End-to-end handling of the reported case

Given:

```text
ENTRY_MODE       : WORKFLOW_ONLY
GOAL_STATE_BEFORE: absent
new files             0
identical collisions  745
DIFFERING collisions  0
```

correct behavior is:

1. mark preliminary authority `pending_write_plan`;
2. keep PHASE 0-2 target-byte read-only;
3. answer PHASE 3 in session evidence even if `_decisions.md` cannot be written;
4. derive final desired workflow bytes;
5. remove every byte-identical no-op;
6. if no byte-changing path remains, run `--plan-empty` and continue verification with
   `not_required:no_target_writes`;
7. if only absent workflow files remain, classify exact CREATE rows and explicit derived MKDIRs;
8. if pristine shipped existing files must change, use `distribution_restore` and source freshness;
9. only if a genuinely customized existing planned path remains, ask Q-ROLLBACK once and bind the
   named unit to those exact paths;
10. proceed to PHASE 4/5 only after the required path-scoped authority exists.

The run must not stop merely because an audit record cannot be persisted.

## 11. Regression requirements

A final release audit must independently test at least:

- preliminary source=target and nested-root behavior;
- zero-write/no-op convergence;
- unrelated collision isolation;
- pending decision/failure/cache/handoff non-blocking behavior;
- blocker threshold excluding observability-only records;
- optional vs required tool failure;
- CORE-33 discovery-time vs PHASE 3 routing;
- CREATE parent MKDIR freshness and external parent drift;
- file target freshness between writes;
- independent distribution restore-source drift;
- named snapshot/VCS continuation and named-unit drift;
- semantic scope separate from rollback safety;
- symlink/junction/hardlink/path-alias/path-shape refusal;
- interview persistence guards;
- process-tree cleanup on validator timeout;
- EN/ID contract parity, documentation links, and flowchart source/render parity.

No finite regression suite proves every possible host behavior. The release claim is limited to the
explicit state space and fixtures executed by the audit, with real safety boundaries retained rather
than bypassed to obtain a green result.

## C2GRAPH v1.2.0 emergency authority

C2GRAPH v1.2.0 adds Bounded Emergency Authorization (BEA). BEA binds a host-mediated human approval
assertion to the existing canonical PHASE 3.W write plan, rollback evidence, target, run/session, exact actions, and
expiry. It does not create a second `ACTUAL_WRITE_SET`, does not widen semantic scope, and is not a generic break-glass
bypass. Host authentication and authoritative incident-state tracking remain outside the repository mechanism. See
`C2GRAPH-EMERGENCY-AUTHORITY-GUIDE.md` and the Indonesian companion guide.
