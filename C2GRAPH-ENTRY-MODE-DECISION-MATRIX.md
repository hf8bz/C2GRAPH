# C2GRAPH Entry-Mode Decision Matrix
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

> Release scope: all entry modes in this matrix ship together in the Version 1 release, `v1.0.0`, dated **12 August 2026**.

> Deterministic routing guide for choosing `ADOPT_WITH_GOAL`, `WORKFLOW_ONLY`, or `BOOTSTRAP` before an agent changes a repository.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) - [Agent instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) - [Reference](C2GRAPH-REFERENCE.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md)

Blocker routing: [C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md).

For installation/adoption decisions, keep the C2GRAPH distribution at `C2GRAPH_SOURCE_ROOT` and
treat the existing project as `TARGET_ROOT`. Reading prompts or scanners from `C2GRAPH_SOURCE_ROOT`
does not authorize copying them into `TARGET_ROOT`. The write barrier is tracked as
`ROLLBACK_AUTHORITY`; while `ROLLBACK_AUTHORITY=pending`, the target remains byte-read-only.

For rollback/write-plan decisions, see [C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md).

## 1. Decision order

Choose the entry mode from repository shape and requested scope in this order. Do not choose it from goal count alone.

### Decision 1 -- Is the repository empty or brand new?

- **Yes:** use `BOOTSTRAP-NEW-REPO.prompt.md`.
- **No:** continue to Decision 2.

### Decision 2 -- Does this session need to change product behaviour?

Product behaviour includes application source, business logic, migrations, product tests, or feature configuration that changes what the product does.

- **Yes:** `WORKFLOW_ONLY` is forbidden. Use a goal-bound path. For existing-repo adoption intended to finish with a new implementation goal, use `SCAN-AND-ADOPT.prompt.md` directly.
- **No:** continue to Decision 3.

### Decision 3 -- Is the requested work only C2GRAPH install, repair, upgrade, or verification?

- **Yes:** use `INSTALL-WORKFLOW-ONLY.prompt.md` with `ENTRY_MODE=WORKFLOW_ONLY`.
- **No:** the request does not fit workflow-only. Identify the real task before choosing an entry point.

## 2. Primary matrix

| Repo state | Requested scope | Goal state | Correct entry | Why |
| --- | --- | --- | --- | --- |
| empty/new | bootstrap project | any/not applicable | `BOOTSTRAP-NEW-REPO.prompt.md` | workflow-only requires an existing non-empty repo |
| existing/non-empty | adopt repo and end with implementation goal | absent | `SCAN-AND-ADOPT.prompt.md` | direct SCAN owns the goal-emitting adoption path |
| existing/non-empty | adopt repo and end with implementation goal | terminal-only | `SCAN-AND-ADOPT.prompt.md` | previous terminal goals do not replace the need for the new objective |
| existing/non-empty | install C2GRAPH only | absent | `INSTALL-WORKFLOW-ONLY.prompt.md` | no goal is required for control-plane installation |
| existing/non-empty | install C2GRAPH only | empty | `INSTALL-WORKFLOW-ONLY.prompt.md` | empty ledger is preserved |
| existing/non-empty | upgrade C2GRAPH only | terminal-only | `INSTALL-WORKFLOW-ONLY.prompt.md` | terminal goals remain terminal |
| existing/non-empty | repair C2GRAPH only | inactive | `INSTALL-WORKFLOW-ONLY.prompt.md` | unresolved goal state is preserved and reported, not repaired through installation |
| existing/non-empty | verify C2GRAPH only | active | `INSTALL-WORKFLOW-ONLY.prompt.md` | existing active goal is preserved but not executed in maintenance session |
| existing/non-empty | repair C2GRAPH only | invalid | `INSTALL-WORKFLOW-ONLY.prompt.md` only if goal records remain untouched | invalid ledger is reported; goal repair is separate |
| existing/non-empty | install C2GRAPH and change product feature | any | not workflow-only; split or use goal-bound work | mixed product work violates the hard scope boundary |
| existing/non-empty and already under C2GRAPH | ordinary product work under a suitable active goal | active | use the normal goal-bound workflow, not an installation entry prompt | entry prompts are for adoption/bootstrap/control-plane installation, not every later product session |

## 3. Goal state is not the mode selector

The following implication is false:

```text
active_goal_count == 0  =>  WORKFLOW_ONLY
```

The correct logic is:

```text
existing_non_empty_repo
AND requested_scope_is_only_C2GRAPH_control_plane
=> ENTRY_MODE=WORKFLOW_ONLY
```

Goal state changes preservation behaviour and final reporting; it does not independently choose the entry mode.

## 4. Goal-state handling matrix inside workflow-only

| Before state | Installation allowed? | Goal mutation allowed? | Product work allowed? | Required result |
| --- | --- | --- | --- | --- |
| `absent` | yes | no | no | keep `.agent/goals/goals.jsonl` absent; a shipped directory/`.gitkeep` alone does not change the classification |
| `empty` | yes | no | no | remain logically empty |
| `active` | yes | no | no in this session | preserve active goal; do not resume it automatically |
| `terminal_only` | yes | no | no | preserve terminal statuses; never reactivate |
| `inactive` | yes | no | no | preserve and report unresolved state |
| `invalid` | conditionally, only without goal mutation | no | no | report invalid state; separate goal-repair task if needed |

## 5. Mixed-request matrix

A workflow-only request becomes invalid when it is mixed with product implementation.

| User request | Workflow-only action |
| --- | --- |
| "Install C2GRAPH." | proceed if repo is existing/non-empty |
| "Upgrade C2GRAPH to the new workflow version." | proceed if only control-plane files are needed |
| "Verify an existing C2GRAPH installation." | proceed |
| "Repair the workflow scripts but leave the app alone." | proceed if repair truly stays inside authorized control-plane scope |
| "Install C2GRAPH and fix checkout." | STOP/split; checkout is product work |
| "Install C2GRAPH and update the DB schema." | STOP/split; migration is product work |
| "Install C2GRAPH and reactivate our last completed goal." | STOP; goal reactivation is forbidden in workflow-only |
| "Install C2GRAPH and fix malformed goal records." | STOP or separate goal-repair task; workflow-only must not mutate logical goals |

## 6. Entry-source matrix

| Agent instruction | Selected meaning | Safe for workflow-only? |
| --- | --- | --- |
| "Read `INSTALL-WORKFLOW-ONLY.prompt.md` and execute it fully as `ENTRY_MODE=WORKFLOW_ONLY`." | explicit workflow-only wrapper | yes |
| "Run `SCAN-AND-ADOPT.prompt.md`." | direct goal-emitting adoption | no |
| "Scan the repo; there is no goal." | ambiguous | no |
| "Install the workflow without a goal." | intent is suggestive but entry source is under-specified | not sufficient by itself |
| full canonical instruction from `C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md` | explicit source, scope, gates, finalization, and STOP rules | preferred |

## 7. Integrity versus authorization matrix

| Command | Session | Zero active goals allowed? | Meaning |
| --- | --- | --- | --- |
| `<C2PY> scripts/goal/gate.py` | workflow-only installation/finalization | yes, if ledger integrity otherwise permits it | integrity check; not product authorization |
| `<C2PY> scripts/goal/gate.py --require-active` | later product implementation | no | active-goal authorization gate |

Never use the second command as a prerequisite for installing workflow machinery. Never treat the first command as authorization to implement product changes.

## 8. PHASE 8 matrix

| Entry mode | PHASE 8 action |
| --- | --- |
| `ADOPT_WITH_GOAL` | use the goal-emitting adoption branch when readiness permits |
| `WORKFLOW_ONLY` | preservation/final-report branch only; emit no goal and change no product behaviour |
| `BOOTSTRAP` | follow the bootstrap contract |

A workflow-only run that reaches a goal-emission path is on the wrong branch and must STOP.

## 9. Final report matrix for workflow-only

| Field | Required value/space | What it proves |
| --- | --- | --- |
| `ENTRY_MODE` | `WORKFLOW_ONLY` | correct branch was selected |
| `GOAL_STATE_BEFORE` | one of six defined states | before-state was classified |
| `GOAL_STATE_AFTER` | one of six defined states | after-state was classified |
| `GOAL_MUTATIONS` | `0` | logical goal plane was not changed |
| `PRODUCT_CHANGES` | `0` | no product behaviour was changed |
| `WORKFLOW_STATUS` | `installed`, `repaired`, `upgraded`, or `verified` | control-plane outcome |
| `PRODUCT_READY` | `no` | workflow-only never authorizes product work in the current session |

## 10. Deterministic STOP and continuation matrix

| Condition | Required outcome |
| --- | --- |
| repo is empty/new | STOP workflow-only; use bootstrap |
| product behaviour must change | STOP workflow-only; move product work to a goal-bound session |
| required semantic write is outside control-plane scope | STOP and name the exact file/task |
| `_decisions.md` cannot currently be persisted | **continue PHASE 3** with ordered `PENDING_DECISION`; persistence is not a blocker |
| failure/cache/discipline/handoff record is pending | continue unless a separate real blocker prevents the required next action |
| preliminary scan shows many identical/differing collisions | continue read-only; final authority waits for `ACTUAL_WRITE_SET` |
| source=target/nested | rollback basis cannot prove existing-file restore; zero-write/create-only plan may still proceed |
| PHASE 3.W `--plan-empty` | `not_required:no_target_writes`; continue read-only verification |
| PHASE 3.W exact CREATE-only plan | `new_files_only` when destinations remain absent |
| planned existing shipped path has independent byte-identical before-image | `distribution_restore` may cover that exact path |
| planned customized existing path lacks before-image | `permission` blocker at PHASE 3.W; obtain snapshot/VCS only for the printed paths |
| planned path is link-like/junction/symlink/path-shape/hardlink unsafe | `gate_red`/unsafe write-plan blocker; re-plan or stop |
| PHASE 4 optional tool fails | record limitation; continue independent approved work |
| PHASE 4 required capability has safe approved fallback | use fallback and narrow claims |
| PHASE 4 required capability has no safe fallback | blocker at PHASE 4 |
| goal mutation is required for workflow verification | STOP and report a separate goal-plane defect |
| PHASE 8 attempts goal emission/reactivation | STOP; wrong branch |
| CORE-33 value exposure in PHASE 0-2 | contain/quarantine; continue only independent safe read-only work; human decision occurs at PHASE 3 |
| CORE-33 mitigation incomplete at PHASE 3 | STOP at PHASE 3 |
| secret exposure after PHASE 3 | contain and return to PHASE 3 before further action |
| unauthorized target mutation occurs before exact authority | STOP; inventory effects and obtain cleanup/rollback authority |
| write-plan fingerprint changes before first mutation | invalidate transaction-start authority and return to PHASE 3.W |
| per-write `--verify-op` fails immediately before a planned mutation | stop before that path is touched and return to PHASE 3.W |
| restore-backed `--expect-source-state` fails | stop before that path is touched; source rollback bytes drifted, return to PHASE 3.W |
| later phase discovers a new write path | return to PHASE 3.W; silent write-set expansion forbidden |

`WRITESET_AUTHORIZATION` is a reason/step label, not an eighth blocker class. An uncovered required
semantic write normally uses blocker class `permission`. Pending observability does not count toward
`blocker_stop_threshold`; only unresolved blockers preventing a required next action count.

### 10A. Rollback candidate matrix

| Exact plan | Source relation/current bytes | Candidate | External snapshot required? |
| --- | --- | --- | --- |
| zero writes | any | `not_required:no_target_writes` | no |
| CREATE-only, destinations absent | any safe target | `new_files_only` | no |
| existing planned MODIFY/DELETE, current bytes equal independent shipped source | independent source | `distribution_restore` | no |
| existing planned MODIFY/DELETE, no independent before-image | any | `snapshot_or_vcs_required` | ask Q-ROLLBACK only for those planned paths; named unit resolves the permission blocker without waiting for the automatic candidate to change |
| desired final bytes already equal target | any | no-op | omit path before rollback classification; never create a fake MODIFY |
| CREATE requires absent parent | any | derived `MKDIR` + `EXPECT_DIR_ABSENT` | verify/create parent top-down as explicit action, then verify CREATE |
| named rollback unit later missing/changed | any | `permission` for dependent path | return to PHASE 3.W before write |
| unsafe link-like/junction/symlink/path-shape/hardlink plan | any | `unsafe_path_refused` | snapshot does not cure path unsafety; re-plan |

Rollback authority and semantic scope are independent. A product path never becomes legal
`WORKFLOW_ONLY` work merely because its rollback candidate is `new_files_only`.

## 11. Future transition matrix

| State after workflow-only | Later objective exists? | Next action |
| --- | --- | --- |
| no active goal | yes | prepare requirements, emit/select a real goal, then pass `goal/gate.py --require-active` |
| valid active goal preserved | yes, and same goal still matches | later session may use that goal after active-goal gate passes |
| terminal-only | yes | create a new goal; do not reactivate a completed/superseded/abandoned goal just for convenience |
| inactive or invalid ledger | yes | resolve goal-plane problem in a separate appropriate task before product work |
| any state | no | no action required; workflow can remain installed without an active goal |

## 12. One-line rule

**Workflow-only may exist without an active goal because it changes the control plane only; product implementation may not.**
## Resource / CLI-continuity matrix

| Condition | Outcome | Continue with | Must not do |
| --- | --- | --- | --- |
| budget below soft boundary | continue | current authorized action | spend continuity reserve early |
| SCAN used >= 288 but < 300 | `suspended_resumable` | capsule + Prompt 97 | start a new work unit |
| BOOTSTRAP used >= 188 but < 200 | `suspended_resumable` | capsule + Prompt 97 | start a new work unit |

A Prompt-97 continuation starts a new execution/resource epoch. Do not feed the exhausted prior-epoch `BUDGET_USED` into the new classifier; reset local usage to `0`, increment `BUDGET_EPOCH`, and carry cumulative usage only for audit.
| hard cap already reached | `suspended_resumable` | text-only capsule from current context | run another tool just to checkpoint |
| CLI/host session closes after PHASE/FASE 3 | `suspended_resumable` | newest transcript/durable capsule + Prompt 97 | restart PHASE/FASE 0 |
| `.agent/state/**` absent but capsule valid | resumable | validate/echo capsule | classify as no checkpoint |
| no capsule and no durable checkpoint survived | reconstruction required | verifiable evidence only | invent completed work |
