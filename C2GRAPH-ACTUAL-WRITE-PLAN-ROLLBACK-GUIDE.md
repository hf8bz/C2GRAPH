# C2GRAPH v1.0.0 - Actual Write Plan & Rollback Authority Guide
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

> Release baseline: this contract is part of the C2GRAPH Version 1 release, `v1.0.0`, dated **12 August 2026**.

> Purpose: prevent false PHASE 3 blockers by computing rollback authority from the exact bytes-changing
> target write plan, not from preliminary collision counts or audit-record persistence.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) - [Safe GATE 0](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md) - [Entry-mode matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md)

## 1. The problem this contract prevents

A preliminary scan can observe hundreds of target paths, but observation is not an intended write.
The number of `new`, `identical`, or `DIFFERING` collisions therefore cannot answer the rollback
question by itself.

A concrete failure pattern is:

```text
new files             0
identical collisions  745
DIFFERING collisions  0
rollback_candidate    new_files_only
rollback_manifest     <empty>
```

followed by a request for an external snapshot merely because the next PHASE 3 answer would normally
be written to `docs/graph-engineering/_decisions.md`.

That state is invalid for three independent reasons:

1. a preliminary collision scan must never claim final write authority;
2. a source/target self-comparison cannot prove an independent before-image;
3. persistence of a PHASE 3 answer is transport/audit state and is not itself a reason to stop the
   decision queue or widen the semantic write plan.

C2GRAPH now separates **rollback basis**, **semantic decisions**, **actual write plan**, and **final
path-scoped rollback authority**.

## 2. Four concepts that must not be merged

### 2.1 `ROLLBACK_BASIS`

Read-only evidence collected before the write plan is known. It may indicate that an independent
C2GRAPH distribution could restore pristine shipped files, that new-file rollback may be possible, or
that the selected source overlaps the target and cannot serve as a before-image.

It is **not permission to write**.

### 2.2 `ACTUAL_WRITE_SET`

The exact target operations that will change bytes after all mutation-affecting human decisions are
known:

```text
create:<relative path>
modify:<relative path>
delete:<relative path>
```

A path whose final desired bytes already equal current target bytes is a no-op and must not appear in
the set.

### 2.3 `ROLLBACK_AUTHORITY`

The rollback method proven for the exact `ACTUAL_WRITE_SET`. Authority is path-scoped. Proving rollback
for path A does not authorize path B.

### 2.4 Semantic scope

Whether C2GRAPH is allowed to change the path at all. Rollback safety never grants semantic scope.
For example, `WORKFLOW_ONLY` may have a rollback-feasible `new_files_only` plan for `src/app.ts`, but
product source is still forbidden in that entry mode.

The path enters `ACTUAL_WRITE_SET` only after **both** semantic scope and intended byte change are
established.

## 3. PHASE 0 preliminary preflight

Run from an independent `C2GRAPH_SOURCE_ROOT` when available:

```bash
<C2PY> \
  "$C2GRAPH_SOURCE_ROOT/scripts/install/preflight.py" \
  --template "$C2GRAPH_SOURCE_ROOT" \
  --target "$TARGET_ROOT"
```

The expected final-authority field is:

```text
rollback_candidate     pending_write_plan
```

Possible rollback-basis observations include:

- `new_files_only_available`;
- `distribution_restore_available`;
- `mixed_requires_write_plan`;
- `unsafe_path_findings`;
- `source_overlap_unusable`.

None is final authority. PHASE 0-2 remain target-byte read-only.

### Why source=target is not final authority

If the source and target are the same directory, every compared path may look identical simply because
it is literally the same file. That comparison proves nothing about an independent rollback copy.
`preflight.py` therefore reports `source_overlap_unusable` rather than manufacturing hundreds of
identical collisions and an empty `new_files_only` manifest.

A source-overlap limitation is not itself a stop for a zero-write run or a create-only plan. It matters
only when a planned existing-file write needs a before-image from the source.

## 4. PHASE 0-2 remain byte-read-only

During PHASE 0-2, keep complete logical records in session/host evidence:

- `PENDING_DECISION`;
- `PENDING_FAILURE`;
- `PENDING_DISCOVERY_CACHE`;
- `PENDING_DISCIPLINE`;
- `PENDING_HANDOFF`;
- `PENDING_SECURITY_INCIDENT`;
- staged tooling maps, generated runbooks, coverage census, and draft documents.

Do not create a target artifact merely because the logical information exists.

This rule eliminates circular authority: the run must not mutate the target in order to ask the human
for permission to mutate the target.

## 5. PHASE 3 questions do not depend on `_decisions.md` persistence

Every PHASE 3 answer is captured immediately as ordered session state. If
`docs/graph-engineering/_decisions.md` is already in the final authorized write set, the answer may be
persisted there. If not, it remains `PENDING_DECISION`.

The next PHASE 3 question proceeds either way.

Therefore this condition is **not a blocker**:

```text
_decisions.md cannot currently be modified
```

unless the actual semantic work itself truly requires modifying that file. Audit persistence alone is
not semantic work.

A run must never say:

```text
cannot advance to the next PHASE 3 question because _decisions.md is unauthorized
```

That is a circular decision-persistence defect.

## 6. Build `ACTUAL_WRITE_SET` after the mutation-affecting answers

At PHASE 3.W:

1. list every intended target byte change;
2. validate each path against entry-mode semantic scope;
3. remove every no-op whose desired bytes equal current bytes;
4. do not add failure/cache/decision/handoff persistence merely to make audit records durable;
5. add a runtime artifact only when it is semantically required for the resulting installed workflow;
6. classify each remaining operation as `create`, `modify`, or `delete`;
7. reject plans in which one planned regular-file path is an ancestor of another planned regular-file path;
8. run plan-specific preflight, which derives missing CREATE-parent directories into the rollback manifest.

Examples:

### Zero-write verification

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" \
  --target "$TARGET_ROOT" \
  --plan-empty
```

Expected:

```text
planned writes         0
rollback_candidate     not_required:no_target_writes
```

No snapshot is required merely because the target contains customized files or because audit records
remain pending.

### Create-only workflow artifact

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" \
  --target "$TARGET_ROOT" \
  --plan-op create:.agent/tooling.yml
```

When the destination is still absent:

```text
rollback_candidate     new_files_only
```

Preflight also prints every missing parent directory as `+ MKDIR <path>` plus
`EXPECT_DIR_ABSENT <path>`, and includes those rows in `target_state_sha256` and
`rollback_manifest_sha256`. A derived directory is a rollback candidate, not proof that C2GRAPH
created it. Execute derived MKDIRs **top-down as separate actions**. Immediately before each one:

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" --target "$TARGET_ROOT" \
  --verify-op mkdir:<relative-directory> --expect-state absent
```

Only after `freshness_verified yes` may that one directory be created, without recursive/implicit
parents. CREATE freshness refuses while an implied parent is absent. On rollback, delete the exact
created files and remove derived MKDIR paths deepest-first only when the same run created them and
they are still empty. If a parent appears before its MKDIR verification, return to PHASE 3.W rather
than claiming ownership of that directory.

### Existing shipped file with independent before-image

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" \
  --target "$TARGET_ROOT" \
  --plan-op modify:docs/graph-engineering/_decisions.md
```

If current target bytes are identical to that path in the unchanged independent distribution:

```text
rollback_candidate     distribution_restore
```

The source hash is bound into the authority, and rollback restores the original bytes.

### Customized existing file that is actually planned for modification

If the target file differs from the independent shipped before-image and the run really plans to
modify/delete it:

```text
rollback_candidate     snapshot_or_vcs_required
```

This is a legitimate blocker **until** a named rollback unit is supplied. Ask for a named external
snapshot/archive or VCS unit for the printed paths only. Do not restart earlier PHASE 3 questions.

The automatic candidate is a diagnosis, not a retry loop. Once the human supplies a unit, capture:

- `ROLLBACK_UNIT` -- stable name/location/revision;
- `RESTORE_OWNER` -- who can execute the restore;
- `COVERED_PATHS` -- exactly the printed `? SNAPSHOT` rows, no implicit widening;
- `ROLLBACK_UNIT_EVIDENCE` -- immutable/read-only fingerprint such as archive SHA-256, revision id, or
  equivalent before-image evidence;
- the `write_plan_sha256` this unit covers.

Then set `ROLLBACK_AUTHORITY=proven:<named-unit>` for those exact paths. Do **not** re-run automatic
preflight expecting `snapshot_or_vcs_required` to disappear: preflight is correctly reporting that
there is no automatic distribution before-image. Before the first dependent write and before every
covered MODIFY/DELETE, re-check the named unit with its unit-specific read-only method. If the unit is
missing, changed, or its coverage cannot be proven, the `permission` blocker remains legitimate and
the path stays unwritten.

## 7. Unrelated collisions are not blockers

Suppose 745 product files exist in the target and none belongs to `ACTUAL_WRITE_SET`.

For a zero-write run they are irrelevant:

```text
--plan-empty
=> not_required:no_target_writes
```

For a plan that creates one new workflow artifact, the unrelated product files are still irrelevant:

```text
--plan-op create:.agent/runtime-created.jsonl
=> new_files_only
```

The rollback question is about planned effects, not the size or messiness of the repository.

## 8. Persistence artifacts never widen semantic work

The following artifacts may remain pending through final reporting when persistence is not required by
the actual installed state:

- `_decisions.md` rows;
- failure and lesson ledger rows;
- tooling inventory rows;
- discovery cache rows;
- discipline rows;
- handoff/checkpoint records;
- interview queue/answer files;
- security-incident records.

They may be persisted only after PHASE 3.W when the exact action is semantically required, included
in `ACTUAL_WRITE_SET`, covered by path-scoped authority, and fresh immediately before persistence.

A pending observability artifact is not a blocker and does not count toward `blocker_stop_threshold`.

## 9. `blocker_stop_threshold` counts only real next-action blockers

The configured threshold is not a count of warnings or failure records. Count only conditions that are:

1. unresolved now; and
2. preventing a required next action.

Do **not** count:

- `PENDING_FAILURE` records;
- claim limits;
- optional-tool failures;
- deferred persistence;
- independent branch limitations;
- already resolved blockers.

`WRITESET_AUTHORIZATION` is a reason/step label, not an eighth blocker class. If an exact required
semantic write lacks rollback coverage, use blocker class `permission` and name the uncovered path.

## 10. PHASE 4 is dependency-aware

A failed tool does not globally stop PHASE 4 merely because it failed.

- optional tool failure -> record limitation; continue independent approved work;
- required tool with safe fallback -> use the approved fallback and narrow claims;
- required tool with no safe fallback -> `tool_absent`/appropriate gate blocker at PHASE 4;
- fetching/installing a new tool -> separate explicit human approval plus tool-specific rollback for
  host/package side effects.

This prevents a product-specific optional tool from blocking workflow installation.

## 11. Path aliasing and unsafe planned writes

Plan-specific preflight fails closed for:

- symlink `TARGET_ROOT`;
- symlink parent redirection;
- incompatible file/directory path shape;
- unreadable target path;
- hardlinked existing target selected for generic modification;
- NTFS alternate-data-stream syntax, trailing dot/space components, or Windows reserved device names;
- plan rows that alias after portable NFC/case normalization;
- one planned regular-file path being an ancestor of another planned regular-file path;
- malformed or duplicate plan paths.

A hardlinked file is unsafe because a write-in-place can change bytes reachable through a second path
that is not in `ACTUAL_WRITE_SET`.

## 12. Freshness before the transaction and before every write

Plan-specific preflight emits aggregate fingerprints and expected pre-states per filesystem action:

```text
EXPECT_DIR_ABSENT <derived parent directory>
EXPECT_ABSENT <CREATE path>
EXPECT_SHA256 <hash> <MODIFY/DELETE path>
EXPECT_SOURCE_SHA256 <hash> <restore-backed path>
```

`EXPECT_SOURCE_SHA256` exists only when rollback depends on the independent distribution source. It
prevents a restore-backed write from proceeding after that source silently changes.

Immediately before the **first** target mutation, re-run the identical plan-specific preflight and
compare `write_plan_sha256`, `target_state_sha256`, `rollback_manifest_sha256`, and
`source_manifest_sha256` for restore-backed plans. Any drift invalidates the transaction-start
authority.

That check is not sufficient for a multi-file plan. Immediately before **every** individual
filesystem action, verify the exact path again. Derived directories are verified/created top-down:

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" --target "$TARGET_ROOT" \
  --verify-op mkdir:.agent --expect-state absent

# create .agent only after the check above succeeds; do not use recursive parent creation

<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" --target "$TARGET_ROOT" \
  --verify-op create:.agent/new-file.yml --expect-state absent

<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" --target "$TARGET_ROOT" \
  --verify-op modify:AGENTS.md --expect-state sha256:<EXPECTED_HASH> \
  --expect-source-state sha256:<EXPECTED_SOURCE_HASH>
```

When the plan emitted `EXPECT_SOURCE_SHA256`, both `source_freshness_verified yes` and
`freshness_verified yes` are required. Otherwise `freshness_verified yes` is required. Named
snapshot/VCS paths additionally require the named rollback-unit evidence to remain accessible and
unchanged. A nonzero exit or `freshness_verified no` stops **before that path is touched** and returns
the run to PHASE 3.W. DELETE uses the same SHA-256 target pre-state form as MODIFY. CREATE must not
implicitly create a missing parent; a missing derived parent is handled by its own verified MKDIR
action. If a later phase discovers a new target write, return to PHASE 3.W and classify the expanded
exact write set; never silently append it.

This reduces multi-file TOCTOU exposure but does not create an OS-level transaction. A change can in
principle occur between a freshness check and the subsequent filesystem mutation. C2GRAPH therefore
retains one-writer-per-path discipline and must not claim mathematically race-free atomicity.

## 13. Expected handling of the reported FASE 3 failure

For a state such as:

```text
new files             0
identical collisions  745
DIFFERING collisions  0
rollback_candidate    new_files_only
rollback_manifest     <empty>
```

use this diagnostic sequence:

1. Treat that old result as non-authoritative preliminary evidence.
2. Verify that source and target are not accidentally the same/nested root.
3. Continue PHASE 3 questions using `PENDING_DECISION`; do not write `_decisions.md` just to advance.
4. After mutation-affecting answers are complete, derive the final desired bytes, remove every
   byte-identical no-op, and calculate exact `ACTUAL_WRITE_SET`.
5. If the set is empty, run `--plan-empty`; proceed with read-only verification.
6. If only new paths are needed, classify those CREATE operations; no external snapshot is required.
7. If an existing pristine shipped path must change, use an independent source and
   `distribution_restore` when byte-identical before-image proof exists.
8. Ask for external rollback authority only for a planned existing path that lacks a proven
   before-image.

The previous `STOP_AT: FASE 3 -- existing-file decision persistence blocked` is therefore not a valid
stop condition by itself.

## 14. Legitimate hard blockers that remain

The goal is not "never stop". The goal is "stop only for a real dependency or safety boundary".
Legitimate examples include:

- `WORKFLOW_ONLY` would need a product/source change;
- a planned path is unsafe/redirected/hardlinked;
- a planned customized existing file has no proven rollback before-image;
- CORE-33 mitigation is incomplete at PHASE 3;
- a required PHASE 4 capability has no approved safe fallback;
- a mandatory verification gate is red;
- target state changes after authority and before write;
- a new write path appears after PHASE 3.W and has not been re-authorized.

None should be bypassed merely to make a run reach PHASE 8.

## 15. Regression matrix required by this contract

At minimum, automated regression must cover:

1. source=target preliminary scan yields `pending_write_plan`, not final authority;
2. source=target + `--plan-empty` succeeds with `not_required:no_target_writes`;
3. runtime CREATE receives exact `new_files_only` authority;
4. pristine existing shipped MODIFY receives `distribution_restore`;
5. customized existing planned MODIFY requires snapshot/VCS;
6. customized existing file outside the plan does not block zero-write or unrelated CREATE;
7. 745 unrelated target files do not affect zero-write/create-only plan classification;
8. parent symlink planned write is refused;
9. hardlinked existing planned MODIFY is refused;
10. pending `_decisions.md` persistence does not stop the PHASE 3 question queue;
11. pending failures above `blocker_stop_threshold` do not count unless they are true blockers;
12. optional PHASE 4 tool failure does not block independent work;
13. required no-fallback PHASE 4 capability does block at PHASE 4;
14. write-set expansion returns to PHASE 3.W;
15. rollback-safe product path remains forbidden in `WORKFLOW_ONLY` because scope is separate;
16. a byte-identical desired result is eliminated as a no-op before rollback classification;
17. every missing CREATE parent emits `EXPECT_DIR_ABSENT`; CREATE refuses until parent MKDIR is
    freshness-verified and explicitly created top-down;
18. an externally appearing derived parent fails its `mkdir:<path>` freshness check;
19. a named snapshot/VCS answer converts only its exact printed paths to
    `ROLLBACK_AUTHORITY=proven:<named-unit>` without requiring automatic preflight to change candidate;
20. a missing/changed named rollback unit blocks only its dependent planned existing-file write;
21. restore-backed source drift fails `--expect-source-state` before the dependent target write;
22. self-test timeout cleanup terminates the complete child process group and returns exit 124 rather
    than hanging the verification run.

## 16. Operator acceptance checklist

Before the first write:

- [ ] Preliminary preflight says `pending_write_plan`, not final authority.
- [ ] Source and target relation is explicit.
- [ ] All PHASE 3 mutation-affecting answers are known.
- [ ] Unpersisted answers are retained as ordered `PENDING_DECISION` rather than blocking the queue.
- [ ] `ACTUAL_WRITE_SET` contains only bytes-changing paths; final desired bytes were computed first
  and every byte-identical no-op was removed.
- [ ] Every path passed semantic-scope validation before rollback classification.
- [ ] Audit-only persistence did not widen the plan.
- [ ] Plan-specific preflight returned a candidate appropriate to the exact operations.
- [ ] External snapshot/VCS is requested only for printed planned existing paths without before-images;
  once named, its exact coverage/evidence is bound to the write plan rather than waiting for preflight
  to change its automatic diagnostic.
- [ ] Every derived `MKDIR` has `EXPECT_DIR_ABSENT`, is verified and created top-down, and CREATE never
  creates a parent implicitly.
- [ ] Aggregate authority is revalidated before first write, every filesystem action passes its own
  `--verify-op` immediately before mutation, and every restore-backed row also verifies
  `--expect-source-state`.
- [ ] Any later new write path returns to PHASE 3.W.
- [ ] Pending failures/claim limits are not counted as blockers merely because they are pending.
- [ ] PHASE 4 stops only for a capability actually required with no safe approved fallback.
