# C2GRAPH Safe GATE 0 & Rollback-Authority Guide
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

> Release baseline: this safety contract is part of the first public C2GRAPH release, `v1.0.0`, dated **12 August 2026**.

> Purpose: prevent discovery, bookkeeping, or "read-only-looking" commands from changing the target
> repository before C2GRAPH has a proven rollback authority.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Workflow-only agent instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) - [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md) - [Reference](C2GRAPH-REFERENCE.md) - [Actual write plan](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md)

Blocker routing: [C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md).

## 1. Why this guide exists

A real failure class motivated this contract: a GATE 0 package-manager probe attempted to discover
`pnpm` through a runner, triggered package resolution, and created `node_modules/.ignored` in an
existing repository that had no `.git` rollback unit. The run then wrote `.agent/failures.jsonl` to
record the failure. Product work never started, but the target had already changed twice before
rollback authority existed.

That outcome is invalid. GATE 0 is now defined by **byte-level effect**, not by intent or command
name. Before rollback authority is proven, the target repository must remain byte-identical.

## 2. Two roots are mandatory

Resolve these roots before doing anything else:

- `C2GRAPH_SOURCE_ROOT`: the unpacked, trusted C2GRAPH distribution containing the entry prompt and
  shipped scanners;
- `TARGET_ROOT`: the existing repository that will receive C2GRAPH.

Do not copy C2GRAPH into `TARGET_ROOT` just so its prompts or scripts become available. The trusted
pre-write scanners and install preflight run from `C2GRAPH_SOURCE_ROOT` and point at `TARGET_ROOT`.

Example:

```bash
C2GRAPH_SOURCE_ROOT=/opt/c2graph/C2GRAPH-en
TARGET_ROOT=/work/acme

<C2PY> \
  "$C2GRAPH_SOURCE_ROOT/scripts/install/preflight.py" \
  --template "$C2GRAPH_SOURCE_ROOT" \
  --target "$TARGET_ROOT"
```

The preflight itself is target-byte read-only. It does not copy, create, delete, rename, fetch,
install, repair, normalize, or clean target paths.

## 3. The rollback-authority barrier

The barrier begins before GATE 0. **PHASE 0 preliminary preflight does not end it.** Final target write
authority is computed only after PHASE 3 has made every mutation-affecting decision and the exact
`ACTUAL_WRITE_SET` is known.

While final authority is unresolved, **PHASE 0-2 keep all target bytes immutable**, including C2GRAPH
bookkeeping. The ban includes:

- application/product files;
- `.agent/failures.jsonl` and `.agent/lessons.jsonl`;
- `.agent/tooling.yml` and `.agent/toolmap.jsonl`;
- `.agent/draft/**`;
- `.agent/state/**`, including lock, handoff, cursor, discipline, and discovery-cache state;
- generated docs, indexes, lockfiles, package-manager artifacts, and target-local temporary files;
- `__pycache__` or `.pyc` created in the target by a Python helper.

Logical records exist as session evidence: `PENDING_DECISION`, `PENDING_FAILURE`,
`PENDING_DISCOVERY_CACHE`, `PENDING_DISCIPLINE`, `PENDING_HANDOFF`, or
`PENDING_SECURITY_INCIDENT`.

Authority is **path-scoped**. Proving rollback for one path does not authorize another. It is also
separate from semantic scope: a rollback-safe path may still be forbidden by `WORKFLOW_ONLY`,
bootstrap scope, or another entry-mode boundary.

## 4. GATE 0 discovery is static S1-S4

GATE 0 may use only evidence that does not execute a candidate project tool:

| Rung | Evidence | Meaning |
| --- | --- | --- |
| `current_process` | the already-running trusted interpreter/process; no new child is launched | only that current runtime is proven executable |
| `S1` | executable name resolved on `PATH` by filesystem lookup, without launching it | `on_path_unverified`; path visibility is not execution proof |
| `S2` | project-local executable/path evidence, inspected without launching the candidate | `present_local_unverified`; local presence is not execution proof |
| `S3` | concrete repository invocation in scripts, CI, hooks, tasks, containers, or documentation | `runner_declared_unverified`; the invocation is declared but has not been executed |
| `S4` | manifest, lockfile, or equivalent static dependency declaration | declared dependency evidence; installation/executability remains unverified |
| `V` | active verification after rollback authority and only when required for the next authorized action | execution may establish runtime usability; `V` is outside GATE 0 discovery |

No `V` execution belongs to pre-authority GATE 0.

## 5. Commands forbidden as discovery probes

Do not execute these merely to determine availability during GATE 0:

```text
npx
npm exec
pnpm exec
pnpm dlx
yarn dlx
bunx
uvx
pipx run
corepack
go run
project-local binaries
arbitrary <tool> --version
arbitrary <tool> --help
network probes or package fetches
```

The list is illustrative, not a loophole: any command capable of hooks, package resolution, cache
creation, lockfile changes, shim bootstrap, workspace initialization, or target writes is active
verification, not static discovery.

A manifest containing `npx playwright test` proves only that the repository declares that call. It
does not prove that `npx` can fetch, resolve, or run Playwright safely on the current host.

## 6. Why `--help`, `--version`, `check`, and `validate` are not automatically safe

Safety is established by **source + exact invocation + observed byte-level effect**. Names do not
confer safety. A wrapper can run install hooks before printing `--version`; a `check` command can
create a state directory; a package-manager shim can bootstrap itself; Git can refresh metadata or
invoke fsmonitor hooks.

C2GRAPH therefore distinguishes:

- **trusted pre-authority allowlist**: shipped source-owned scanners/preflight whose relevant
  invocation has been audited as target-byte read-only;
- **active verification**: any candidate tool execution, deferred until authority exists and the
  current phase actually needs it;
- **mutating action**: requires explicit write/rollback authority even when its command name sounds
  diagnostic.

`scripts/state/lock.py check` is specifically byte-read-only: it does not create `.agent/state/` and
reports a stale lease without deleting it. `acquire` and `release` are explicitly mutating.

## 7. Git discovery is static before authority

Pre-authority discovery does **not launch a Git executable at all**. `scripts/scan/census.py` reads only
safe, in-tree Git metadata such as a regular `.git/HEAD` and a non-symlink loose ref that cannot escape
the target. Linked-worktree `.git` indirection, symlinked metadata, worktree cleanliness, commit count,
and other facts requiring Git execution remain `unknown` before authority.

This rule exists because `GIT_OPTIONAL_LOCKS=0`, disabled fsmonitor, `--help`, or a read-looking Git
subcommand cannot make an arbitrary `git` shim/wrapper trustworthy. The pre-authority boundary is
stronger: no Git child process is started.

For an existing Git worktree, Git may become a rollback authority only after the human explicitly
attests that the current worktree/HEAD is the intended rollback point. That attestation opens an
authorized verification boundary, not a write by itself: run the exact Git status/revision `V` before
the first target write. If `V` contradicts the attestation, STOP without writing and obtain another
authority.

A repository without `.git` is not automatically blocked. `new_files_only` preflight authority can
For planned CREATE paths, preflight derives missing parent directories as exact `MKDIR` fingerprint/rollback rows and emits `EXPECT_DIR_ABSENT`. These are separate top-down filesystem actions: verify each with `--verify-op mkdir:<path> --expect-state absent`, create one directory only, then run CREATE freshness. Recursive/implicit parent creation is forbidden.
cover exact new paths. An overwrite/merge needs a named snapshot/archive created outside `TARGET_ROOT`
or another external rollback unit. **Do not run `git init` to manufacture rollback authority**: that
would mutate the protected target before authority exists and make the bootstrap circular.

## 8. Install preflight has two distinct stages

### 8.1 PHASE 0: preliminary rollback basis

Run preflight without `--plan-op` or `--plan-empty`. The expected authority field is always:

```text
rollback_candidate     pending_write_plan
```

The scan may report a rollback-basis observation such as `new_files_only_available`,
`distribution_restore_available`, `mixed_requires_write_plan`, `unsafe_path_findings`, or
`source_overlap_unusable`. Those observations are inventory only. **Do not translate collision counts
into write permission.**

If source and target are the same or nested, the source is unusable as an independent before-image.
That fact does not stop a zero-write run; it means existing-file restore has not been proven.

### 8.2 PHASE 3.W: classify the exact actual write plan

After all mutation-affecting PHASE 3 answers are known, construct only the paths whose bytes really
must change. Validate semantic scope before rollback classification. No-op paths are omitted.

Run either:

```bash
<C2PY> scripts/install/preflight.py --template "$C2GRAPH_SOURCE_ROOT" --target "$TARGET_ROOT" --plan-empty
```

or repeat `--plan-op`:

```bash
<C2PY> scripts/install/preflight.py \
  --template "$C2GRAPH_SOURCE_ROOT" \
  --target "$TARGET_ROOT" \
  --plan-op create:.agent/tooling.yml \
  --plan-op modify:docs/graph-engineering/example.md
```

Final candidates are:

### `not_required:no_target_writes`

The exact final write set is empty. No rollback unit is required for target bytes. Pending audit
records do not invalidate this result.

### `new_files_only`

Every actual planned target write is `CREATE` and each destination is still absent. Rollback deletes
only those exact created files and removes new parent directories only when empty.

It does not authorize modifying an existing file and does not widen semantic scope.

### `distribution_restore`

At least one planned existing-file `MODIFY`/`DELETE` has a byte-identical before-image in an
**independent, unchanged** C2GRAPH source distribution. Exact planned CREATE paths may coexist in the
same authority. Rollback deletes CREATE paths and restores RESTORE paths from the bound source hash.

### `snapshot_or_vcs_required`

At least one **actual planned existing-file write** lacks a proven before-image. Request broader
rollback authority only for the printed planned paths. Unrelated differing collisions do not matter.

### `unsafe_path_refused`

A planned path is unsafe or internally inconsistent. Current guards cover target-root/parent symlinks,
path-shape conflicts, unreadable targets, malformed plan paths, and hardlinked existing targets whose
write-in-place effect could escape the declared one-path write set.

For the complete state machine and examples, see
[C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md).

## 9. TOCTOU: freshness is checked twice at different scopes

Preflight is not a permanent permit. Immediately before the first target mutation, rerun the
**identical plan-specific preflight** and compare `write_plan_sha256`, `target_state_sha256`,
`rollback_manifest_sha256`, and `source_manifest_sha256` for restore-backed plans. If any bound state
changes, the previous authority is invalid and the run returns to PHASE 3.W.

The plan also emits `EXPECT_ABSENT` for CREATE, `EXPECT_SHA256` for MODIFY/DELETE, and
`EXPECT_SOURCE_SHA256` for restore-backed existing-file rows. Immediately before **each individual
mutation**, run `preflight.py --verify-op ACTION:PATH --expect-state ...`. When a source expectation
exists, append `--expect-source-state sha256:<source_hash>` and require source freshness too. Require
`freshness_verified yes`. A failed path check stops before that mutation and returns to PHASE
3.W. This per-write check matters after earlier writes in the same plan have already changed the
aggregate target fingerprint.

If a later phase discovers a new path to create/modify/delete, it also returns to PHASE 3.W. Silent
write-set expansion is forbidden. Per-write verification narrows the race window; it is not an OS
transaction lock, so one-writer-per-path discipline remains mandatory.

## 10. Path aliasing and path-shape rules

Fail closed when a **planned** write would use:

- a symlink `TARGET_ROOT`;
- a source selected through a symlink when it is being used as restore evidence;
- a target parent symlink;
- a destination that is a directory/non-regular path where a file operation is expected;
- an unreadable existing target;
- a hardlinked existing file for generic modification;
- a junction/link-like directory redirect where supported by the host;
- NTFS alternate-data-stream/trailing-dot-space/reserved-name path grammar;
- portable Unicode/case aliases or regular-file ancestor/descendant plan conflicts;
- malformed/duplicate relative plan paths.

A hardlink matters because modifying one inode in place may change bytes reachable through another
path not listed in `ACTUAL_WRITE_SET`.

Preliminary source overlap is different: source=target/nested means "unusable as independent restore
evidence," not "stop all read-only work." A final zero-write or create-only plan can still be safe.

## 11. Pending evidence and decision transport

A pre-authority failure does not justify writing a failure ledger. A PHASE 3 answer does not justify
writing `_decisions.md` merely so the next question can be asked. The same separation applies to
cache, discipline, handoff, interview answers, tooling rows, and security incidents.

Rules:

1. capture the complete logical record in session/host evidence first;
2. continue the decision queue even when `_decisions.md` is not writable;
3. persist only after PHASE 3.W when the exact action is semantically required, in `ACTUAL_WRITE_SET`,
   path-scoped authorized, and fresh immediately before persistence;
4. persistence-only records never widen semantic work and never become blockers by themselves;
5. `blocker_stop_threshold` counts only unresolved blockers that prevent a required next action, not
   `PENDING_FAILURE` records or claim limits.

If compaction threatens while persistence is uncovered, keep a complete `PENDING_HANDOFF`. If the host
cannot preserve required session evidence, that is a `resource` problem; do not create target state
without authority.

## 12. Validator boundary: C2GRAPH corpus vs product corpus

C2GRAPH's control-plane validators must not judge arbitrary product files as if they were C2GRAPH
artifacts. The shipped `.agent/distribution-files.txt` is the explicit distribution manifest used to
bound default corpus validation. Product scanning remains the adoption scanner's job.

This prevents unrelated product content from causing false C2GRAPH failures, for example:

- product Markdown with its own placeholders or intentionally broken draft links;
- product JSONL that does not use a C2GRAPH ledger schema;
- product Python/scripts increasing C2GRAPH distribution counts;
- product files changing claims such as "168 shipped files".

## 13. If an unsafe probe already mutated the repository

Do **not** immediately delete the side effect merely because it appears to be probe-generated. Cleanup
is itself a mutation and may destroy pre-existing user data.

Use this sequence:

1. STOP the current run.
2. Inventory every observed changed/created path and hash/read enough evidence to distinguish a new
   side effect from a pre-existing path.
3. Report `TARGET_MUTATIONS` with exact observed paths; when complete coverage is unavailable, report `unknown`; do not report zero.
4. Record the failure as `PENDING_FAILURE` if rollback authority is still pending.
5. Obtain a named rollback/cleanup authority: known snapshot/VCS unit, or explicit human approval for
   the exact proven side-effect paths.
6. Revert only the paths proven to belong to the unsafe probe.
7. Verify cleanup independently. Re-run preflight for destination freshness, but do not mistake `target_state_sha256` for a whole-repository digest.
8. Re-run static GATE 0 and install preflight from `C2GRAPH_SOURCE_ROOT`.
9. Continue only under fresh authority.

For the concrete `node_modules/.ignored` incident, do not assume the path was created by the probe
solely because it appeared afterward. Confirm its pre-run absence/evidence first; then remove it only
under explicit rollback/cleanup authority.

## 14. Stopped-run report

A workflow-only run that stops before successful finalization uses the stopped schema, not the
seven-field success schema. The authority field now distinguishes preliminary basis from final plans:

```text
ENTRY_MODE        : WORKFLOW_ONLY
RUN_OUTCOME       : stopped
STOP_AT           : <phase/gate + exact STOP rule or blocker>
GOAL_STATE_BEFORE : <observed state>
GOAL_STATE_AFTER  : <same | not_rechecked>
GOAL_MUTATIONS    : 0
PRODUCT_CHANGES   : 0
TARGET_MUTATIONS  : 0 | unknown | reverted | <exact remaining paths>
ROLLBACK_AUTHORITY: pending_write_plan |
                    not_required:no_target_writes |
                    proven:new_files_only:<sha256> |
                    proven:distribution_restore:<sha256> |
                    proven:<named-unit>
WORKFLOW_STATUS   : not_started | staged | installed_unverified | repair_partial | upgrade_partial
PRODUCT_READY     : no
PENDING_FAILURES  : <count + ids/session labels>
PROGRESS          : phases <done>/<total>; current <phase/gate>; verified units <done>/<total>
NEXT_SAFE_ACTION  : <one concrete action that does not cross the failed gate>
```

Do not invent percentages. Keep failure class and blocker class separate. `WRITESET_AUTHORIZATION` is
a reason/step label, not an eighth blocker class; an uncovered required semantic write is normally a
`permission` blocker.

## 15. Regression acceptance matrix

The release criteria are not satisfied unless automated tests cover at least:

1. fake destructive `pnpm` is discovered without execution;
2. PHASE 0 preliminary scan reports `pending_write_plan`, never final authority;
3. source=target preliminary scan reports unusable restore basis rather than trivial identical authority;
4. source=target `--plan-empty` succeeds with no rollback unit;
5. runtime/generated CREATE receives exact `new_files_only` authority;
6. pristine shipped existing MODIFY receives `distribution_restore` from an independent source;
7. customized existing planned MODIFY requires snapshot/VCS;
8. customized existing file outside the plan does not block zero-write/unrelated CREATE;
9. hundreds of unrelated target files do not affect plan-specific zero-write/create classification;
10. target-root and parent symlink paths are refused;
11. directory/file type collision is refused;
12. hardlinked existing target modification is refused;
13. plan fingerprint drift invalidates earlier authority;
14. fake destructive Git is never launched by pre-authority census;
15. `state/lock.py check` remains byte-read-only;
16. pending PHASE 3 decisions advance without `_decisions.md` persistence;
17. pending failures do not count toward blocker threshold merely because they exist;
18. optional PHASE 4 tool failure does not block independent work;
19. required PHASE 4 capability without safe fallback does block;
20. rollback-safe product paths remain forbidden by `WORKFLOW_ONLY` semantic scope.

## 16. Operator acceptance checklist

Before allowing the first target write, verify all are true:

- [ ] `C2GRAPH_SOURCE_ROOT` and `TARGET_ROOT` are explicit; source overlap is recorded honestly.
- [ ] PHASE 0 preliminary preflight returned `pending_write_plan`, not final write authority.
- [ ] PHASE 0-2 changed zero target bytes.
- [ ] GATE 0 used static S1-S4 only; no candidate package/runner/Git/help/version/network probe executed.
- [ ] All mutation-affecting PHASE 3 answers are known; uncovered persistence remains `PENDING_*`.
- [ ] `_decisions.md` persistence was not required merely to ask the next question.
- [ ] `ACTUAL_WRITE_SET` contains only genuine byte-changing operations.
- [ ] Every path passed semantic-scope checks independently of rollback safety.
- [ ] Plan-specific preflight returned `not_required`, `new_files_only`, `distribution_restore`, or a
      real blocker for the exact planned path set.
- [ ] An external snapshot/VCS unit is requested only for planned existing paths lacking before-images.
- [ ] Unsafe aliases/path shapes/hardlinks are not bypassed.
- [ ] `blocker_stop_threshold` counts only true blockers preventing a required next action.
- [ ] PHASE 4 optional failures are claim limits unless the current required action has no safe fallback.
- [ ] Final plan fingerprints are revalidated before first write; each mutation passes `--verify-op`, and restore-backed rows also pass `--expect-source-state`, immediately before that path is touched.
- [ ] Any later new write path returns to PHASE 3.W.
- [ ] Any stopped run reports exact target mutations and exact completed/total progress.

## Secret-prone input and CORE-33 routing

GATE 0/PHASE 0-2 read-only does not mean arbitrary raw reads are safe. `.env*`, `wrangler*.toml`,
credential files, and other secret-prone inputs use `scripts/scan/redact.py` or are skipped. If a raw
value nevertheless reaches context before PHASE 3, contain/quarantine immediately and stage a sanitized
`PENDING_SECURITY_INCIDENT`; do not emit a stopped run at the discovery line. Continue only independent
safe read-only discovery to the mandatory PHASE 3 human blocker. See
[C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md).
