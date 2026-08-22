# C2GRAPH Workflow-Only Guide
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

> Version 1 release status: `WORKFLOW_ONLY` remains included in C2GRAPH `v1.2.0`; it was already present in the historical v1.1.0 release. 

> Canonical operator guide for installing, repairing, upgrading, or verifying the C2GRAPH control plane in an existing non-empty repository without creating or requiring a new goal.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Release README](README.md) - [Deep dive](C2GRAPH-DEEP-DIVE.md) - [Reference](C2GRAPH-REFERENCE.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md) - [File map](C2GRAPH-FILE-MAP.md) - [Bounded read](C2GRAPH-BOUNDED-READ-GUIDE.md) - [Agent instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) - [Entry-mode matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) - [Safe GATE 0](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) - [Actual write plan](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md)

Blocker routing: [C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md).

## 1. Purpose

`WORKFLOW_ONLY` exists for one narrow lifecycle operation: modify the C2GRAPH workflow/control plane in an **existing, non-empty repository** while leaving product behaviour and the logical goal plane unchanged.

Use it when the requested work is only one or more of the following:

- install C2GRAPH into an existing repository;
- repair a damaged or incomplete C2GRAPH installation;
- align an existing C2GRAPH installation;
- verify that an existing C2GRAPH installation is internally consistent.

The repository does **not** need an active goal for those control-plane operations. A missing goal, an empty goal ledger, only terminal goals, or an inactive goal state does not by itself block workflow installation.

`WORKFLOW_ONLY` is not permission to implement product work without a goal. It is a deliberately narrow exception to the active-goal requirement because the session is forbidden from changing product behaviour.

## 2. Source of truth and precedence

The executable source of truth is the repository-root file:

```text
INSTALL-WORKFLOW-ONLY.prompt.md
```

This guide explains that prompt. It does not replace it. If this guide and the executable prompt ever disagree, the executable prompt wins and the documentation drift must be reported.

`INSTALL-WORKFLOW-ONLY.prompt.md` delegates the common scan/adoption pipeline to `SCAN-AND-ADOPT.prompt.md` under `ENTRY_MODE=WORKFLOW_ONLY`. The delegation does **not** make direct execution of `SCAN-AND-ADOPT.prompt.md` equivalent. Direct SCAN selects `ENTRY_MODE=ADOPT_WITH_GOAL`.

## 3. The three entry modes

| Repository/request state | Correct entry | Goal behaviour | Product changes |
| --- | --- | --- | --- |
| existing non-empty repo; adoption should end with an implementation goal | `SCAN-AND-ADOPT.prompt.md` | may prepare lineage and emit/apply a goal only after readiness | only under the resulting active goal |
| existing non-empty repo; install/repair/upgrade/verify C2GRAPH only | `INSTALL-WORKFLOW-ONLY.prompt.md` | preserve existing goal plane; emit nothing | forbidden |
| empty or brand-new repo | `BOOTSTRAP-NEW-REPO.prompt.md` | bootstrap flow owns its own goal lifecycle | governed by bootstrap contract |

Do not choose `WORKFLOW_ONLY` merely because no active goal exists. Choose it only when the requested change is limited to the workflow/control plane.

## 4. Canonical operator command to the agent

For a file-aware coding agent whose working directory is `TARGET_ROOT`, keep the unpacked C2GRAPH package at a separate `C2GRAPH_SOURCE_ROOT` and send the instruction from [C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md). Do not copy the distribution into the target merely to make the prompt available. The minimum safe form is:

```text
Keep C2GRAPH outside the target as `C2GRAPH_SOURCE_ROOT`; define the existing project as `TARGET_ROOT`. Read `C2GRAPH_SOURCE_ROOT/INSTALL-WORKFLOW-ONLY.prompt.md` in full and execute it from its first instruction through its final STOP condition. Do not bulk-copy the distribution into the target before rollback authority. This request is strictly `ENTRY_MODE=WORKFLOW_ONLY`. Do not execute `SCAN-AND-ADOPT.prompt.md` as a standalone entry point. Do not create or mutate a goal, and do not change product behaviour. Execute every delegated phase, gate, preservation check, and STOP rule. In PHASE 8 use only the WORKFLOW_ONLY finalization branch. Report all seven required final status fields, then stop without beginning product work.
```

Do not shorten this to "run SCAN" or "install C2GRAPH without a goal." Those phrasings omit the entry source and can select the wrong branch or leave the scope boundary under-specified.

## 5. Preconditions before the agent writes anything

Before the first write, the wrapper must establish read-only evidence about the goal plane.

It must:

1. record whether `.agent/goals/goals.jsonl` exists;
2. hash every pre-existing regular file under `.agent/goals/`;
3. run `<C2PY> scripts/goal/gate.py` in integrity-only mode if that script already exists; and
4. record `GOAL_STATE_BEFORE` from the observable ledger state.

If C2GRAPH is not installed yet and `scripts/goal/gate.py` is absent, absence of that script is expected. The file-existence and hash snapshot remains the authoritative pre-write evidence until the newly installed gate can classify the ledger during finalization.

The snapshot step is read-only. The agent must not create `.agent/goals/`, normalize a goal record, repair a malformed goal, or rewrite goal metadata while producing before-state evidence.

## 5A. Mutation-free GATE 0, preliminary basis, and PHASE 3.W authority

`ROLLBACK_AUTHORITY` is not decided by PHASE 0 collision counts. PHASE 0 runs a preliminary,
target-byte-read-only basis scan and leaves final authority as `pending_write_plan`. PHASE 0-2 remain
byte-read-only and all bookkeeping stays as complete `PENDING_*` session evidence.

Tool discovery uses static S1-S4 only. Pre-authority Git discovery launches no `git` executable.
Existing Git can become a rollback unit only after human worktree/HEAD attestation and authorized `V`;
`git init` is never used to manufacture authority inside the protected target.

PHASE 3 asks mutation-affecting questions without requiring `_decisions.md` persistence. Each answer is
retained as ordered `PENDING_DECISION` and the next question proceeds. Only when the decisions make the
intended target effects knowable does PHASE 3.W build `ACTUAL_WRITE_SET`.

The set contains only genuine bytes-changing `create|modify|delete` paths. No-op paths and audit-only
persistence are omitted. Semantic scope is checked separately before rollback classification: a
rollback-safe product path remains forbidden in `WORKFLOW_ONLY`.

Plan-specific outcomes are:

- `not_required:no_target_writes` -- verify read-only; no target rollback unit is needed;
- `new_files_only` -- exact planned CREATE paths plus preflight-derived missing-parent `MKDIR` rows. Each missing parent emits `EXPECT_DIR_ABSENT`, is freshness-checked and created explicitly top-down before the file CREATE; rollback removes it deepest-first only if this run created it and it remains empty;
- `distribution_restore` -- planned CREATE paths plus exact existing shipped paths with independent,
  byte-identical before-images;
- `snapshot_or_vcs_required` -- only actual planned existing paths without before-images need broader
  authority. After Q-ROLLBACK names an external snapshot/archive or VCS unit, bind its exact path
  coverage/evidence to the write plan as `proven:<named-unit>`; do not wait for the automatic candidate
  to change, and re-check that unit before each dependent existing-file write;
- `unsafe_path_refused` -- fix/re-plan an unsafe path; do not bypass path aliasing protection.

Unrelated differing collisions do not block. Source=target cannot prove an existing-file before-image,
but it also does not block a zero-write/create-only plan. Re-run the identical plan-specific preflight
immediately before first mutation and compare all bound fingerprints. Retain the emitted
`EXPECT_ABSENT` / `EXPECT_SHA256` rows plus restore-backed `EXPECT_SOURCE_SHA256`, then require
`--verify-op mkdir:PATH --expect-state absent` for each derived parent, then
`--verify-op ACTION:PATH --expect-state ...` for each file action
immediately before every individual write. A failed freshness check or newly discovered path returns
to PHASE 3.W before that path is touched. This narrows, but does not eliminate, an OS-level race.

See [C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) for the pre-authority
boundary and [C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md)
for exact authority classification and the reported `_decisions.md` blocker case.

## 5B. CORE-33 secret exposure routes to PHASE 3

A secret exposure during delegated PHASE 0-2 does not produce a stopped workflow-only report at the
discovery line. Contain it immediately, quarantine the unsafe raw slice, stage sanitized
`PENDING_SECURITY_INCIDENT` evidence outside target bytes when authority is pending, use
`scripts/scan/redact.py` for secret-prone structural inspection, and continue only independent safe
read-only discovery. PHASE 3 is the mandatory human CORE-33 stop/decision. If mitigation is incomplete,
stop there. Exposure after PHASE 3 returns to PHASE 3 before any further action. See
[C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md).

## 6. Goal-state classification

The workflow-only wrapper recognizes exactly these observable before/after states:

| State | Exact meaning | Required workflow-only behaviour |
| --- | --- | --- |
| `absent` | `.agent/goals/goals.jsonl` does not exist | continue; do not create a logical goal record |
| `empty` | the log exists but projects zero goals | continue; preserve the logically empty state |
| `active` | one or more valid active goals exist without objective conflict | preserve them; do not execute, resume, replace, or alter them in this session |
| `terminal_only` | every projected goal is `completed`, `superseded`, or `abandoned` | preserve terminal status; never reactivate a terminal goal to satisfy installation |
| `inactive` | goals exist, none is active, and at least one is non-terminal | preserve the unresolved state and report it; do not "fix" it through workflow installation |
| `invalid` | projection or ledger invariants fail | workflow repair may continue only if goal records remain untouched; report the invalid state and keep product work blocked |

`GOAL_STATE_AFTER` must have the same semantic meaning as `GOAL_STATE_BEFORE` for a valid workflow-only run.

## 7. Allowed write scope

The session may write only the control-plane surfaces authorized by the delegated SCAN contract. In practical terms, allowed writes are limited to:

- C2GRAPH control-plane artifacts;
- generated adoption/workflow documentation;
- approved tooling installation or repair;
- the rulebook target explicitly permitted by `SCAN-AND-ADOPT.prompt.md`.

The `.agent/goals/` directory may appear because the C2GRAPH control-plane template ships that directory and its `.gitkeep`. Directory creation is not logical goal creation. Adding or changing a `GOAL-*.md` file or a logical goal/status event is a goal mutation and is forbidden.

## 8. Forbidden write scope

During `ENTRY_MODE=WORKFLOW_ONLY`, the agent must not change:

- application or product source code;
- business logic;
- database or data migrations;
- product tests;
- feature configuration that changes product behaviour;
- logical goal records or goal status events.

The agent must not create, adopt, inherit, reactivate, supersede, abandon, complete, or otherwise mutate a goal.

If a required workflow repair would force a product change or goal mutation, the correct result is STOP with a blocker report. The agent must not widen scope silently.

## 9. Delegated SCAN execution

After the wrapper establishes `ENTRY_MODE=WORKFLOW_ONLY` and captures the before-state evidence, it executes `SCAN-AND-ADOPT.prompt.md` **in full under that mode**.

The mode changes only two things:

1. the permitted write scope is restricted to workflow/control-plane work; and
2. PHASE 8 uses the `WORKFLOW_ONLY` finalization branch instead of a goal-emitting branch.

The mode does not skip repository scanning, human-decision points, verification, blocker handling, evidence gates, preservation checks, or STOP conditions.

The scanner's pre-verification goal inventory must reuse the wrapper's before-state snapshot. It must not take a later mutated state and mislabel it as the original state.

## 10. Integrity gate versus product authorization gate

These two commands have different meanings and must not be conflated.

### During workflow-only installation

```bash
<C2PY> scripts/goal/gate.py
```

This is the integrity-only form. A valid zero-active-goal state may pass. It answers whether the goal ledger is structurally/internally acceptable for the checks the script performs; it does not authorize product implementation.

### Before later product implementation

```bash
<C2PY> scripts/goal/gate.py --require-active
```

This is the active-goal authorization form. It must pass in the later goal-bound session before product implementation begins.

Using `--require-active` to decide whether workflow-only installation may run is incorrect. Omitting `--require-active` before later product work is also incorrect.

## 11. PHASE 8 finalization

When delegated SCAN reaches PHASE 8 under `ENTRY_MODE=WORKFLOW_ONLY`, the agent must execute only the workflow-only branch.

The PHASE 8 workflow-only branch has these explicit prohibitions:

- The agent must not run goal readiness for the purpose of manufacturing a goal.
- The agent must not emit a `/goal` block.
- The agent must not write a `GOAL-*.md` file.
- The agent must not append a goal or status event to `goals.jsonl`.
- The agent must not resume an already-active goal automatically.
- The agent must not reactivate a terminal goal.

It must perform the preservation checks and produce the required final report.


### Stopped-run mutation claim

`target_state_sha256` is the install-preflight fingerprint of C2GRAPH destination states. It prevents a
TOCTOU write against a stale preflight, but it is not a hash of the entire target repository. If an
unauthorized command ran, report exact observed paths or `TARGET_MUTATIONS: unknown`; never infer zero
mutation solely from the preflight fingerprint.

## 12. Required final report

The final report contains exactly these seven status concepts:

```text
ENTRY_MODE       : WORKFLOW_ONLY
GOAL_STATE_BEFORE: absent | empty | active | terminal_only | inactive | invalid
GOAL_STATE_AFTER : absent | empty | active | terminal_only | inactive | invalid
GOAL_MUTATIONS   : 0
PRODUCT_CHANGES  : 0
WORKFLOW_STATUS  : installed | repaired | upgraded | verified
PRODUCT_READY    : no
```

A successful workflow-only run must prove all of the following:

- `ENTRY_MODE` is `WORKFLOW_ONLY`;
- `GOAL_STATE_AFTER` preserves the semantic before-state;
- every pre-existing goal-file hash still matches unless a human separately changed that file outside this workflow-only task;
- `GOAL_MUTATIONS` is `0`;
- `PRODUCT_CHANGES` is `0`;
- `WORKFLOW_STATUS` describes the control-plane outcome;
- `PRODUCT_READY` does not falsely claim that installing the workflow authorized product work.

## 13. Stop conditions

The executable wrapper currently defines nine workflow-only STOP rules. They are intentionally narrow:

1. repo is empty/new;
2. requested work includes product/feature behaviour;
3. a required semantic write is outside workflow/control-plane scope;
4. goal mutation would be required for verification;
5. PHASE 8 attempts goal emission/reactivation;
6. PHASE 3.W contains an unsafe planned path, a planned existing path still lacks a before-image, or a
   bound plan fingerprint changes;
7. GATE 0 discovery would require executing/fetching/bootstrapping a candidate tool -- stop that action,
   not independent safe work;
8. an unauthorized target mutation occurs while final authority is unresolved; or
9. CORE-33 exposure is unresolved at PHASE 3 (pre-PHASE-3 exposure is contained and routed there).

The following are **not** standalone STOP conditions:

- `_decisions.md` cannot currently be persisted;
- a failure/cache/discipline/handoff record remains `PENDING_*`;
- preliminary scan found many identical/differing collisions outside `ACTUAL_WRITE_SET`;
- source overlaps target but the final plan is zero-write/create-only;
- an optional product tool is absent or fails while an independent safe path remains.

A STOP result identifies the exact blocker and path/dependency. It must not silently convert the
session into another entry mode or ask for a broad snapshot when only audit-record persistence is
uncovered.

## 14. What happens after workflow-only succeeds

Workflow installation ends without starting product implementation.

If there is no suitable active goal and a new objective later exists, requirements must first become ready. Then a later session may run:

```text
prompts/08-goal-emit.prompt.md
```

The emitted goal is pasted into the host's goal mechanism according to the goal contract. Product work begins only in a separate goal-bound session after:

```bash
<C2PY> scripts/goal/gate.py --require-active
```

passes.

If a valid active goal already existed before the workflow-only session, that goal remains untouched. A later session may use it if it is still the correct objective, but the installation session must not resume it automatically.

## 15. Scenario walkthroughs

### Scenario A: existing repo, no `.agent/goals/` directory

Use workflow-only. Snapshot records an absent ledger and zero pre-existing goal files. Installation may create shipped control-plane directories, including `.agent/goals/.gitkeep`, but may not create a logical goal record. Because `absent` is defined by the non-existence of `.agent/goals/goals.jsonl`, creating only the shipped directory or `.gitkeep` does not change that classification. `GOAL_STATE_AFTER` remains `absent`; no synthetic goal or ledger is allowed.

### Scenario B: existing repo, empty ledger

Use workflow-only. Preserve the empty logical ledger. Do not insert a placeholder goal merely to satisfy a gate.

### Scenario C: existing repo, all previous goals completed

Use workflow-only. Preserve every terminal status. Do not reactivate the last completed goal. A new implementation objective, if one appears later, gets its own goal in a later session.

### Scenario D: existing repo, valid active goal already exists

Workflow-only is still valid if the requested task is strictly C2GRAPH maintenance. Preserve the active goal but do not execute it during installation. Product work remains a separate session.

### Scenario E: existing repo, malformed/conflicting goal ledger

Classify the goal plane as `invalid`. Workflow repair may proceed only if it can avoid mutating goal records. Report the invalid state. Goal repair is a separate task; product work remains blocked.

### Scenario F: user asks "install C2GRAPH and fix this product bug"

Do not combine the requests under workflow-only. The product change makes the session out of scope. STOP or split the work so workflow installation completes separately and the bug fix runs later under an appropriate active goal.

### Scenario G: repository is empty/new

Do not use workflow-only. Use `BOOTSTRAP-NEW-REPO.prompt.md`.

### Scenario H: rerunning workflow-only after a successful installation

The run may classify its outcome as `verified`, `repaired`, or `upgraded` depending on observed changes. It must remain idempotent with respect to product behaviour and logical goal records.

## 16. Common incorrect interpretations

| Incorrect interpretation | Why it is wrong | Correct rule |
| --- | --- | --- |
| "No active goal means workflow-only automatically." | Entry mode is determined by requested scope, not by goal count. | Use workflow-only only for C2GRAPH control-plane work. |
| "Direct SCAN can notice there is no goal and switch modes." | Direct SCAN is the goal-emitting adoption entry. | Enter through `INSTALL-WORKFLOW-ONLY.prompt.md`. |
| "A completed goal can be reopened temporarily for installation." | That falsifies goal history. | Preserve terminal goals unchanged. |
| "The workflow may fix a malformed goal ledger because that is control-plane data." | The wrapper explicitly protects logical goal records. | Report the defect and repair it in a separate goal-repair task. |
| "Installing the workflow means product work is now authorized." | Installation validates/creates control-plane machinery only. | Later product work requires `goal/gate.py --require-active`. |
| "Creating `.agent/goals/.gitkeep` creates a goal." | The directory is control-plane structure, not a logical goal. | Logical goal creation means adding goal records/events. |

## 17. Operator completion checklist

Before accepting a workflow-only run as complete, confirm every box:

- [ ] The target repository was existing and non-empty.
- [ ] The request was limited to C2GRAPH install/repair/upgrade/verification.
- [ ] The agent entered through `INSTALL-WORKFLOW-ONLY.prompt.md`.
- [ ] `ENTRY_MODE=WORKFLOW_ONLY` was explicit.
- [ ] Goal-plane evidence was captured before the first write.
- [ ] Direct SCAN was not used as the standalone entry.
- [ ] All delegated phases/gates ran; no phase was skipped merely because there was no active goal.
- [ ] No product/application behaviour changed.
- [ ] No logical goal record/status changed.
- [ ] PHASE 8 used only the workflow-only finalization branch.
- [ ] The final report contains all seven required fields.
- [ ] `GOAL_MUTATIONS: 0`.
- [ ] `PRODUCT_CHANGES: 0`.
- [ ] Any invalid/inactive goal condition was reported rather than silently repaired.
- [ ] The session stopped after workflow completion and did not begin product implementation.

## 18. Related documentation

- [C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) -- exact operator-to-agent text and acceptance contract.
- [C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) -- deterministic mode selection and edge-case matrix.
- [C2GRAPH-REFERENCE.md](C2GRAPH-REFERENCE.md) -- rule/gate reference.
- [C2GRAPH-TROUBLESHOOTING.md](C2GRAPH-TROUBLESHOOTING.md) -- failure diagnosis.
- [C2GRAPH-DEEP-DIVE.md](C2GRAPH-DEEP-DIVE.md) -- architectural rationale.
- [C2GRAPH-FILE-MAP.md](C2GRAPH-FILE-MAP.md) -- shipped repository paths.
## Python launcher and budget-safe continuation

Workflow-only inherits the same `C2PY` and resource-continuity contracts as delegated SCAN. Resolve `C2PY` once per host/session and replace the notation before every shipped Python CLI. A missing literal `python3` command is not evidence that Python is absent.

SCAN's 300-call cap reserves the final 12 calls for continuity. At/inside that reserve, stop scheduling new work and emit `RUN_OUTCOME: suspended_resumable` plus `C2GRAPH_RESUME_CAPSULE v1`. The wrapper also requires a fresh capsule after PHASE/FASE 3/3.W before PHASE/FASE 4 even when budget is healthy. This provides a portable continuation anchor so a replacement CLI can resume without intentionally repeating GATE 0 or the human interview when the capsule is present, intact, and accepted by the resume contract. Resume through Prompt 97; absence of target `.agent/state/**` is not itself a blocker. The resumed CLI opens a new resource epoch: local `BUDGET_USED` resets to `0`, `BUDGET_EPOCH` increments, and cumulative usage is carried only for audit, so an exhausted prior CLI cannot trap the workflow in repeated `HARD_EXHAUSTED`.
