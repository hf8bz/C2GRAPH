# C2GRAPH Workflow-Only - Agent Instruction
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

> First-public-release status: this operator instruction targets the `WORKFLOW_ONLY` entry point included in C2GRAPH `v1.0.0`, released **12 August 2026**.

> Copy-paste contract for an agent that must install, repair, upgrade, or verify C2GRAPH in an existing non-empty repository without requiring or creating a goal.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) - [Safe GATE 0 guide](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) - [Actual write plan](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md) - [Secret safety](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md) - [Entry-mode matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md)

Blocker routing: [C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md).

## 1. Required layout

Keep the unpacked C2GRAPH distribution **outside** the target repository until write authority is proven.
Define:

- `C2GRAPH_SOURCE_ROOT` = trusted unpacked C2GRAPH distribution;
- `TARGET_ROOT` = existing, non-empty project repository receiving the workflow.

Do not copy C2GRAPH into `TARGET_ROOT` just so the agent can read the prompt.

## 2. Canonical copy-paste instruction

Send the entire block below as one instruction. The agent may have its shell working directory at
`TARGET_ROOT`, but it must read/execute C2GRAPH pre-write material from `C2GRAPH_SOURCE_ROOT`.

```text
This is a C2GRAPH workflow-only installation/repair/upgrade/verification task.

Resolve two explicit roots and the host launcher before doing anything:
- C2GRAPH_SOURCE_ROOT = the trusted unpacked C2GRAPH distribution containing INSTALL-WORKFLOW-ONLY.prompt.md.
- TARGET_ROOT = the existing, non-empty repository receiving C2GRAPH.
- `<C2PY>` below is notation, not an assignment or executable name. Replace it before execution with one exact invocation prefix: POSIX `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`; Windows cmd.exe `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`; or PowerShell `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`.

Read C2GRAPH_SOURCE_ROOT/INSTALL-WORKFLOW-ONLY.prompt.md in full and execute it from its first instruction through its final STOP condition. Treat this request strictly as ENTRY_MODE=WORKFLOW_ONLY.

ENTRY SOURCE AND MODE
- INSTALL-WORKFLOW-ONLY.prompt.md is the required standalone entry point.
- Do not copy the C2GRAPH distribution into TARGET_ROOT merely to make the prompt/scripts available.
- Do not run SCAN-AND-ADOPT.prompt.md as a standalone entry point. It may run only because the workflow-only wrapper delegates to it while preserving ENTRY_MODE=WORKFLOW_ONLY.
- Do not switch to ADOPT_WITH_GOAL or BOOTSTRAP because the goal ledger is absent, empty, terminal_only, inactive, active, or invalid.
- Do not create/reactivate a goal merely to satisfy a gate.

PRE-AUTHORITY ZERO-MUTATION BARRIER
- Until rollback authority is proven, TARGET_ROOT must remain byte-identical.
- Run trusted C2GRAPH pre-write scanners/preflight from C2GRAPH_SOURCE_ROOT with TARGET_ROOT passed explicitly.
- GATE 0 discovery is static S1-S4 only. Do not execute a candidate package manager, project runner, ephemeral runner, Corepack shim, project-local binary, arbitrary <tool> --version, arbitrary <tool> --help, or network probe merely to discover availability.
- In particular, do not execute npx, npm exec, pnpm exec, pnpm dlx, yarn dlx, bunx, uvx, pipx run, corepack, go run, or an executable git for GATE 0 discovery. Git evidence before authority is limited to safe static .git/HEAD/in-tree ref metadata; cleanliness and commit count stay unknown until authorized V.
- Existing Git can be selected as rollback authority only after the human attests the current worktree/HEAD and authorized V confirms it before first target write. For non-Git overwrite/merge, use an external named snapshot/archive or other external rollback unit. Never run git init to manufacture authority inside TARGET_ROOT.
- Do not create .agent/failures.jsonl, .agent/tooling.yml, .agent/draft/**, .agent/state/**, caches, lockfiles, generated docs, __pycache__, or any other target artifact merely to record pre-authority progress.
- Keep pre-authority bookkeeping as PENDING_FAILURE, PENDING_DISCOVERY_CACHE, PENDING_DISCIPLINE, or PENDING_HANDOFF in session/transcript evidence.
- A command is not considered read-only merely because it is named check/validate/help/version. Safety is based on audited source + exact invocation + byte-level effect.

ROLLBACK PREFLIGHT AND ACTUAL WRITE PLAN
- PHASE 0 preflight is preliminary rollback-basis inventory only. Run `<C2PY> C2GRAPH_SOURCE_ROOT/scripts/install/preflight.py` against TARGET_ROOT without --plan-op/--plan-empty and require rollback_candidate=pending_write_plan.
- Do not infer final authority from counts of new, identical, or DIFFERING collisions. A source=target/nested relation is unusable as an independent before-image but is not a blocker for a run that ultimately writes nothing.
- PHASE 0-2 write zero target bytes. Keep decisions/failures/tooling/cache/discipline/handoff/drafts as ordered PENDING_* session evidence.
- PHASE 3 questions continue even when docs/graph-engineering/_decisions.md cannot be persisted. Capture each answer as PENDING_DECISION and ask the next question. Decision persistence is not a blocker.
- After all mutation-affecting PHASE 3 answers are known, build exact ACTUAL_WRITE_SET with only bytes-changing create|modify|delete paths. Omit no-ops.
- Validate semantic scope before rollback classification. Rollback safety never turns a product path into a legal WORKFLOW_ONLY path.
- Run plan-specific preflight with repeated --plan-op ACTION:RELATIVE_PATH, or --plan-empty when there are zero target writes.
- not_required:no_target_writes means no target rollback unit is required; do not create audit files merely to make the run look persisted.
- new_files_only authorizes only exact planned CREATE paths that are still absent, including semantically required new runtime artifacts. Preflight derives every missing parent as `MKDIR` + `EXPECT_DIR_ABSENT`; verify each parent with `--verify-op mkdir:<path> --expect-state absent`, create it top-down as one explicit directory action, and never let CREATE recursively create an implicit parent. Rollback removes a derived directory only when this run actually created it and it is still empty.
- distribution_restore authorizes exact planned CREATE paths plus planned existing MODIFY/DELETE paths whose current bytes have a byte-identical before-image in an independent unchanged C2GRAPH source distribution.
- snapshot_or_vcs_required applies only to printed planned existing-file paths that lack a proven before-image. Ask Q-ROLLBACK only for those paths; do not restart earlier PHASE 3 questions. After the human supplies a named external snapshot/archive or VCS unit, record exact covered paths, restore owner, immutable/read-only unit evidence, and write_plan_sha256, then set `ROLLBACK_AUTHORITY=proven:<named-unit>` for those paths. This is a continuation overlay: do not wait for automatic preflight to stop reporting snapshot_or_vcs_required. Re-check the named unit before every dependent existing-file write.
- unsafe_path_refused means the actual plan is unsafe/inconsistent. Do not bypass symlink, path-shape, unreadable-target, hardlink, or malformed-plan protection.
- Pending decision/failure/cache/discipline/handoff persistence never widens ACTUAL_WRITE_SET and does not count as a blocker by itself.
- Re-run the identical plan-specific preflight immediately before first mutation and compare write_plan_sha256, target_state_sha256, rollback_manifest_sha256, and source_manifest_sha256 when restore-backed. Retain EXPECT_DIR_ABSENT / EXPECT_ABSENT / EXPECT_SHA256 and restore-backed EXPECT_SOURCE_SHA256 rows. Immediately before each derived MKDIR, run `--verify-op mkdir:PATH --expect-state absent`, then create that one directory top-down. Immediately before each file action run `--verify-op ACTION:PATH --expect-state absent|sha256:<hash>`. When EXPECT_SOURCE_SHA256 exists, append `--expect-source-state sha256:<source_hash>`. Named snapshot/VCS paths also re-check their named rollback unit. Any freshness failure or new write path returns to PHASE 3.W before the path is touched; the guard narrows TOCTOU but is not an OS transaction lock.

PRE-WRITE GOAL PRESERVATION
- Before the first write, perform the wrapper's read-only goal-plane snapshot.
- Record whether TARGET_ROOT/.agent/goals/goals.jsonl exists and SHA-256 every pre-existing regular file under TARGET_ROOT/.agent/goals/.
- If the target already has `scripts/goal/gate.py`, run `<C2PY> scripts/goal/gate.py` in integrity-only mode without `--require-active`.
- If it is absent because C2GRAPH is not installed, existence + hashes remain authoritative before-state evidence; classify again with the installed gate during finalization.
- Do not create, normalize, repair, rewrite, inherit, reactivate, supersede, abandon, complete, or otherwise mutate a logical goal.

HARD WRITE SCOPE
- Writes are limited to the C2GRAPH control plane, generated adoption/workflow documentation, explicitly approved tooling action, and the rulebook target authorized by delegated SCAN.
- Do not modify application/product source, business logic, migrations, product tests, or behaviour-changing feature configuration.
- Creating the shipped .agent/goals/ directory/.gitkeep as structure is not logical goal creation; creating or changing goal records is forbidden.

DELEGATED PIPELINE
- Execute the complete SCAN-AND-ADOPT.prompt.md pipeline under ENTRY_MODE=WORKFLOW_ONLY.
- Do not skip a phase, human decision, blocker rule, verification, evidence gate, preservation check, or STOP rule.
- Keep the rollback-authority barrier active until authority is proven; early discovery/cache/failure/handoff/discipline records stay pending rather than becoming target writes.

CORE-33 SECRET-SAFE DISCOVERY AND PHASE 3 ROUTING
- Do not raw-read `.env*`, `wrangler*.toml`, credential/secret files, or a config already suspected of carrying a secret. Use `<C2PY> C2GRAPH_SOURCE_ROOT/scripts/scan/redact.py --target TARGET_ROOT --path <relative-path>` for structural line plus identifier-class/fingerprint evidence.
- If a secret value nevertheless reaches context during PHASE 0-2, do not repeat it. Quarantine the affected raw source/slice, stage sanitized PENDING_SECURITY_INCIDENT in session/host evidence, and continue only independent already-proven-safe read-only discovery.
- Do not emit RUN_OUTCOME=stopped or STOP_AT=PHASE 0/1/2 for CORE-33 secret exposure alone. Carry every pending security incident to PHASE 3 as a mandatory human blocker with no autonomous default.
- At PHASE 3 the human must establish mitigated, false_positive, or mitigation_incomplete. If mitigation is incomplete, STOP at PHASE 3. If exposure occurs after PHASE 3, contain immediately and return to PHASE 3 before any further action.
- This phase routing is specific to CORE-33 secret exposure. Other independent STOP rules remain in force at their own boundaries.

RESOURCE BUDGET AND CLI-SESSION CONTINUITY
- Resource exhaustion is never a terminal task verdict by itself. SCAN uses cap 300 with reserve 12 (soft boundary 288); inherited BOOTSTRAP uses cap 200 with reserve 12 (soft boundary 188).
- Classify before scheduling another tool call with `<C2PY> scripts/budget/check.py --cap <cap> --used <used> --reserve 12 --handoff-cost 8`.
- `HANDOFF_NOW` and `HARD_EXHAUSTED` require `RUN_OUTCOME=suspended_resumable`, never `RUN_OUTCOME=stopped` merely because budget is insufficient. On Prompt-97 resume, increment `BUDGET_EPOCH`, reset local `BUDGET_USED` to `0`, and carry `BUDGET_CUMULATIVE_USED` only for audit.
- Emit/refresh `C2GRAPH_RESUME_CAPSULE v1` after PHASE/FASE 3/3.W and before PHASE/FASE 4 even when budget is still healthy. This capsule is user-visible/session evidence and needs no target write.
- If the CLI closes after that boundary, resume with `prompts/97-budget-resume.prompt.md`, re-resolve C2PY on the new host/session, validate the newest capsule when file-backed, and continue from `NEXT_ACTION`. Do not restart PHASE/FASE 0 or re-ask settled PHASE/FASE 3 decisions.
- At a hard exhausted cap, schedule no new tool solely to checkpoint; emit the capsule from evidence already in context. Missing `.agent/state/**` does not invalidate a transcript capsule.

GOAL GATE SEMANTICS
- Workflow-only installation uses <C2PY> scripts/goal/gate.py in integrity-only mode when applicable.
- Do not use --require-active to decide whether workflow installation may proceed.
- A later, separate product session must pass <C2PY> scripts/goal/gate.py --require-active before product implementation.

PHASE 8
- Execute only the WORKFLOW_ONLY finalization branch.
- Do not emit a /goal block, write GOAL-*.md, append goal/status events, apply inheritance lineage, auto-resume an active goal, or reactivate a terminal goal.

SUCCESS REPORT
Return all seven fields exactly, with PRODUCT_READY always no in this workflow-only session:
ENTRY_MODE       : WORKFLOW_ONLY
GOAL_STATE_BEFORE: absent | empty | active | terminal_only | inactive | invalid
GOAL_STATE_AFTER : absent | empty | active | terminal_only | inactive | invalid
GOAL_MUTATIONS   : 0
PRODUCT_CHANGES  : 0
WORKFLOW_STATUS  : installed | repaired | upgraded | verified
PRODUCT_READY    : no

If a non-resource STOP rule fires before successful finalization, do not force the success schema. Resource budget/context/rate/session boundaries use the `suspended_resumable` schema above, not the stopped-run schema. Return the stopped-run schema from INSTALL-WORKFLOW-ONLY.prompt.md, including RUN_OUTCOME, STOP_AT, TARGET_MUTATIONS, ROLLBACK_AUTHORITY, PENDING_FAILURES, exact completed/total PROGRESS, and NEXT_SAFE_ACTION.

WRAPPER STOP RULES
1. TARGET_ROOT is empty/new -> STOP and use BOOTSTRAP-NEW-REPO.prompt.md.
2. Requested work includes product/feature behaviour -> STOP; workflow-only cannot authorize it.
3. A required write is outside declared workflow/control-plane scope -> STOP and name the exact file/task.
4. Goal records must be mutated to make verification pass -> STOP and report a separate goal-plane defect.
5. PHASE 8 attempts goal emission/reactivation -> STOP; wrong branch.
6. PHASE 3.W plan-specific preflight reports `unsafe_path_refused`, a planned existing-file path without a proven before-image remains `snapshot_or_vcs_required`, or a bound write-plan fingerprint changes -> STOP before that write and re-plan/obtain authority. Preliminary/unrelated collisions and uncovered `_decisions.md` persistence are not this blocker.
7. GATE 0 discovery would require executing/fetching/bootstrapping a candidate tool -> STOP that action before launch; record static evidence/claim limits. Product-tool uncertainty alone is not a workflow-install blocker when the current C2GRAPH phase has a safe alternative.
8. Any target mutation occurs while ROLLBACK_AUTHORITY=pending -> STOP the run, inventory exact paths, report them in TARGET_MUTATIONS, and obtain authority before cleanup/recovery.
9. A CORE-33 secret exposure occurs in PHASE 0-2 -> contain/quarantine immediately but do not globally stop at the discovery line; route the mandatory human decision to PHASE 3. Post-PHASE-3 exposure returns to PHASE 3 before further action.

If an unsafe probe already created something such as `node_modules/.ignored` or an early `.agent/failures.jsonl`, do not silently delete it. Inventory exact observed paths/evidence first, obtain explicit rollback/cleanup authority, revert only proven side effects, verify cleanup independently, then restart from static GATE 0 plus fresh preflight. If complete mutation coverage is unavailable, report `TARGET_MUTATIONS: unknown`; `target_state_sha256` is only a destination-state freshness fingerprint and is not a whole-repository digest.

Do not invent progress percentages. Report exact completed/total phase/gate/unit counts. Keep failure class separate from blocker class.

Stop after C2GRAPH is installed/repaired/upgraded/verified. Do not begin product implementation in this session.
```

## 3. Optional operator preflight command

The operator or agent can prove the write boundary before installation:

```bash
C2GRAPH_SOURCE_ROOT=/absolute/path/C2GRAPH-en
TARGET_ROOT=/absolute/path/existing-repo

<C2PY> \
  "$C2GRAPH_SOURCE_ROOT/scripts/install/preflight.py" \
  --template "$C2GRAPH_SOURCE_ROOT" \
  --target "$TARGET_ROOT"
```

For the Indonesian package, replace `C2GRAPH-en` with `C2GRAPH-id`.

Do not follow this command with a blind `cp -a`, `cp -f`, `rsync --delete`, or archive extraction over
the target. The wrapper owns the authorized installation set and preservation logic.

## 4. Host cannot read C2GRAPH source files

Paste the full `INSTALL-WORKFLOW-ONLY.prompt.md` from `C2GRAPH_SOURCE_ROOT`; do not paste a shortened
summary. Clipboard helpers are operator conveniences only and do not change the target repository.

```bash
pbcopy < "$C2GRAPH_SOURCE_ROOT/INSTALL-WORKFLOW-ONLY.prompt.md"       # macOS
xclip -selection clipboard < "$C2GRAPH_SOURCE_ROOT/INSTALL-WORKFLOW-ONLY.prompt.md"  # X11
wl-copy < "$C2GRAPH_SOURCE_ROOT/INSTALL-WORKFLOW-ONLY.prompt.md"      # Wayland
```

```powershell
Get-Content -Raw "$env:C2GRAPH_SOURCE_ROOT\INSTALL-WORKFLOW-ONLY.prompt.md" | Set-Clipboard
```

## 5. Operator acceptance checks

Reject the run as incorrect if any is false:

- `C2GRAPH_SOURCE_ROOT` and `TARGET_ROOT` were explicit and C2GRAPH was not bulk-copied before authority.
- No unauthorized candidate command ran before authority; if one did, mutation coverage is reported as exact paths or `unknown`, never inferred from `target_state_sha256`.
- No `npx`/package-manager/runner/local-binary/help/version/network/Git-process discovery probe executed at GATE 0.
- No early ledger/draft/cache/state/lock/generated artifact was persisted merely to record progress.
- PHASE 0 preflight remained `pending_write_plan`; PHASE 3.W produced a final candidate for the exact `ACTUAL_WRITE_SET`; aggregate fingerprints were revalidated before first write and each mutated path passed per-write freshness immediately before mutation.
- `ENTRY_MODE` remained exactly `WORKFLOW_ONLY` throughout delegated SCAN.
- All seven success fields exist only if successful finalization was actually reached.
- `PRODUCT_READY` is `no` in the workflow-only session.
- `GOAL_MUTATIONS=0` and `PRODUCT_CHANGES=0` on success.
- A stopped run uses exact `TARGET_MUTATIONS`, rollback state, pending failures, completed/total progress, and one next safe action.
- The agent stopped without beginning product implementation.

## 6. Later product work

Start product work only in a separate goal-bound session. Create a new goal only when a real new
objective exists; if a suitable active goal already exists, use it according to its scope. In either
case, product write requires:

```bash
<C2PY> scripts/goal/gate.py --require-active
```

Only exit 0 authorizes product implementation.
