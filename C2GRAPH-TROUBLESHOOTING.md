
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.
## `python3` is not found / Windows returns exit 9009

Do not repair this by installing a package manager, using `npx`, or repeatedly probing interpreter names. C2GRAPH commands use the host-resolved `C2PY` launcher:

- POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
- Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`; PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`

Then replace `<C2PY>` in the documented command with that exact prefix. The wrappers validate Python 3.8+ and select supported host spellings. `<C2PY>` itself is never typed literally. Resolve it again after a CLI/session/host restart. If no supported Python runtime exists, classify `tool_absent` only when the next required C2GRAPH action needs Python; do not bootstrap Python autonomously.

## `budget insufficient` after PHASE 3, then the CLI closes

This is not a terminal STOP. The expected result is `RUN_OUTCOME: suspended_resumable`. A `C2GRAPH_RESUME_CAPSULE v1` must already have been emitted/refreshed after PHASE/FASE 3/3.W and before PHASE/FASE 4. In the new CLI, paste that capsule into `prompts/97-budget-resume.prompt.md`, re-resolve `C2PY`, and continue from the exact `NEXT_ACTION`.

Do not restart PHASE/FASE 0, do not re-run settled discovery, and do not re-ask settled PHASE/FASE 3 questions merely because `.agent/state/**` does not exist. If the previous run reached the hard cap before a durable target handoff could be written, the transcript capsule is still a valid checkpoint. Only if neither a capsule nor a durable checkpoint survived may state be reconstructed from verifiable evidence, labelled `CHECKPOINT_SOURCE: reconstructed`.

Mechanical budget boundaries are: SCAN 300 hard / 288 soft; BOOTSTRAP 200 hard / 188 soft. `<C2PY> scripts/budget/check.py` classifies them **per execution/resource epoch**. Prompt 97 opens a new epoch on a resumed CLI: increment `BUDGET_EPOCH`, reset local `BUDGET_USED` to `0`, and preserve `BUDGET_CUMULATIVE_USED` only for audit. Reusing the exhausted prior-epoch local counter is a resume-loop defect.

## `RUN_OUTCOME: stopped` at PHASE 1 because a secret was exposed

**Symptom:** the agent reports `STOP_AT: PHASE 1.x / CORE-33 secret exposure`, asks for credential
rotation, and ends the run before the PHASE 3 decision gate.

**Cause:** stale CORE-33 semantics treated containment and global stopping as the same operation.

**Correct behavior:** do not repeat the value; quarantine the raw source/slice; stage sanitized
`PENDING_SECURITY_INCIDENT`; use `scripts/scan/redact.py` for secret-prone structural inspection;
continue only independent safe read-only discovery; then make PHASE 3 the mandatory human decision.
If mitigation is incomplete, stop **at PHASE 3**. Exposure after PHASE 3 returns to that barrier.

Do not infer that every other STOP rule moved to PHASE 3. This routing is specific to CORE-33 secret
exposure. See [C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md).

# C2GRAPH - troubleshooting and FAQ

> Version scope: troubleshooting on this page targets C2GRAPH `v1.2.0`. Historical v1.1.0 checksums remain useful only when validating an upgrade baseline.

> **C2GRAPH** is short for **Convert to Graph Engineering**.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Release README](README.md) - [Deep dive](C2GRAPH-DEEP-DIVE.md) - [Reference](C2GRAPH-REFERENCE.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md) - [File map](C2GRAPH-FILE-MAP.md) - [Bounded read](C2GRAPH-BOUNDED-READ-GUIDE.md) - [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) - [Agent instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) - [Entry-mode matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md)

Blocker routing: [C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md).

The failures people actually hit, and what each one means. Symptoms first, because a symptom is what you
have when something goes wrong.

---

## The run wrote a handoff and then did nothing

It wrote the checkpoint correctly and stopped at the echo. That stop is a setting, not a bug:
`handoff_autoresume` in `.agent/config.yml`. It ships as `true`, so a run that stops anyway is
telling you one of five things, and the echo says which. `DIVERGENCE` is not `none` - the handoff
and the working tree disagree, and that is the one case where a human has to look. `BOUNDARY` is not
`none` - the next action would cross a human input gate, an installation, a sensitive action, a red
gate, or the end of adoption, and `autoresume_crosses_phase_boundary` is `false`. `DEFERRED` is
above `autoresume_deferred_max`. The handoff has no `## Unattended continuation` section, so nothing
authorised the resume. Or the file is simply set to `false` in your copy. Read the echo before
changing the setting: four of those five are the setting working.

## The run kept asking questions after resuming

Check what it asked. `autoresume_question_policy` is `blocker_or_sensitive_only`, which permits
exactly two kinds of interruption while unattended: a blocker with no written default, and approval
for a sensitive or destructive action. A question about naming, ordering, or a preference that
already carries a recommendation is a policy violation, and the run should have taken the default
and recorded `DEFERRED-nn` instead. A question already answered under `## Decisions taken` is a
different failure: that is re-asking, forbidden in every mode, and it means the rehydrate did not
read the handoff it claims to have read. Both are worth reporting, because both are the model
ignoring a written rule rather than the rule being absent.

## The run asked me a pile of questions at once

Verify the archive and source tree against the v1.2.0 checksums, then run aggregate verification before changing
workflow semantics. Open `AGENTS.md` and confirm that the source identifies C2GRAPH `v1.2.0`. A mismatched or
locally modified tree can carry different prompt behavior, including instructions that send every question in one
message.

If the verified release tree still behaves this way, the agent is disobeying a written rule rather than following
one. Point it at the PHASE 3 rules and the gate that checks them. The rule is stated in the prompt, in the
prohibitions list, and in a gate, so all three can be quoted back.

---

## The interview asked me nothing at all

That is a success, not a failure. When the draft stage settles everything, there is nothing left that
the repository could not answer. The run should say so explicitly and report three counts: how many
values were derived from your code, how many took a template default, and how many needed confirmation.
If the last count is zero, the interview is legitimately empty.

What you should still do is read the `DEFAULTED` list once. Those are values your repository did not
contain, so they came from the template, and they are the ones most likely to be wrong for you.

---

## The interview script will not run on my host

The canonical PHASE 3 interview does **not** require `scripts/interview/ask.py`. Questions and answers
stay in host/chat session evidence until PHASE 3.W has formed the exact `ACTUAL_WRITE_SET`; this avoids
a circular requirement to write a queue merely to discover which paths are allowed to be written.
A host-native in-memory batch UI may collect the ordered answers in one interaction; otherwise the
agent asks one question per message. Neither path writes `TARGET_ROOT`.

`<C2PY> scripts/interview/ask.py` remains an optional utility for an already-persisted queue. `--emit` is
read-only. Its answer-writing mode requires **both** `--write-authorized` and
`--post-plan-authorized`, and the caller may use those flags only after PHASE 3.W has placed the exact
answers destination in `ACTUAL_WRITE_SET`, semantic scope permits it, path-scoped authority covers it,
and per-action freshness is green. Generic snapshot authority alone is insufficient because rollback
safety is not semantic scope. The agent must never create a
queue, answers file, or `_decisions.md` merely to keep the interview moving.

---

## The agent refuses to run the scan scripts

The fix is a capability split, not a blanket permission to execute. C2GRAPH distinguishes audited
read-only scanners from active or mutating commands. A command name such as `check`, `validate`,
`--help`, or `--version` does not prove byte safety; neither does the operator's intention.

At GATE 0 the agent may use the C2GRAPH scanners whose exact invocation is documented and covered by
mutation-safety fixtures, plus static S1-S4 evidence. It must not launch candidate package managers,
ephemeral runners, project-local binaries, arbitrary help/version commands, or network probes merely
to discover availability. Active verification is step `V`: it occurs only after rollback authority,
only when the capability is actually needed, and with its expected writes/network/cache effects known.

Some scripts under `scripts/` are deliberately mutating (`install`, lock acquire/release, authorized
interview persistence, and other control-plane writers), so "it lives under scripts" is never sufficient
permission. If the agent refuses an invocation documented as audited read-only, point it to that exact
script contract and its mutation regression test. If an invocation is not on that audited path, treat
its write behaviour as unknown until source plus invocation are reviewed; do not execute it to find out.

---

## The run asked me something that is obviously in my code

That is a real defect and worth reporting. Every value proven from the repository is supposed to be
marked `DERIVED` and cited with a `path:line` in `.agent/draft/_derivation.md`, and re-asking anything
already marked that way is forbidden. Ask the run to name the search it performed before marking the
value as needing confirmation. If it cannot name one, it skipped the work.

---

## I answered six questions and then my session died

Resume rather than restart. Every answer is captured as it arrives. Before PHASE 3.W it stays
in session/host evidence. After PHASE 3.W it is persisted only when the exact destination is semantically
required, listed in `ACTUAL_WRITE_SET`, and covered by path-scoped authority plus current freshness;
otherwise it remains pending. Resume from the first unanswered question using whichever checkpoint surface
is valid. Starting over is not merely slower; it risks a different answer to a question already settled.

---

## `.agent/draft/` is still sitting in my repo after the run

The run did not finish PHASE 5. The draft is supposed to be promoted into place and then deleted, and a
gate checks exactly that. Do not delete it by hand first: read it, because it tells you what would have
been installed. Then ask the run to complete the promotion, which includes asserting that no
`NEEDS_CONFIRMATION` marker survives anywhere in the installed files.

---

## A `NEEDS_CONFIRMATION` marker ended up in a real file

That means a draft value was promoted without being resolved. Treat every such value as unset rather
than configured. Search for the marker, recover answers from authorized `_decisions.md` records and ordered
`PENDING_DECISION` session evidence where available, and re-run only whatever remains unanswered. A marker left in `.agent/config.yml` is more dangerous than a
missing key, because tooling will read it as a value.

---

## The agent claims tests pass but nothing ran

This is what the tool inventory in PHASE 0 exists to prevent. What the agent may claim is bounded by
what its tools can prove, so if no test runner is available on the machine, the correct report is that
it cannot know. Check `.agent/tooling.yml` for what was actually found, and check whether the claim
cites an artifact. A claim without an artifact violates the fifth invariant.

---

## The router keeps getting longer

There is a hard limit, `router_max_lines`, set to 210 for v1.2.0. The v1.2.0 release carries `AGENTS.md` at 205 lines in English and 204 lines in Indonesian. When
the router grows, something that belongs in a scope or a runbook was written into it. That cost is paid
on every single request, so the limit is a budget rather than a style preference. Move the new rule down
a layer and leave a routing row behind.

---

## My context filled up in the middle of a long run

Expected, and handled. Context-pressure thresholds trigger a logical checkpoint before the host can close the
session. The checkpoint may be a durable `CURSOR.json`/`HANDOFF.md` **or** a validated
`C2GRAPH_RESUME_CAPSULE v1` in transcript/external evidence; target-state persistence is not required for
continuity. In a new CLI, use Prompt 97, open a fresh resource epoch, preserve cumulative usage for audit,
and resume from the capsule's exact `NEXT_ACTION`. Do not re-run completed discovery or re-ask settled
PHASE 3 decisions.

---

## Two subagents both edited the same file

That is a partition problem, not a concurrency problem. Lanes are supposed to be disjoint, and the
selectivity probes in PHASE 2 exist to prove that a query lands inside one lane instead of smearing
across all of them. Re-check the partitions in `.agent/config.yml`, and look for a lane that is really
two lanes wearing one name.

---

## Which edition should I install, English or Bahasa Indonesia?

Whichever your team reads without translating. The two trees are structurally identical: the same 123
paths, the same rule identifiers, the same gates, the same numbers. Only the prose differs, and the
machine-read field names stay in English in both editions on purpose, because a validator cannot afford
a translated key.

Do not install both. Two rulebooks means two answers to the same question.

---

## Can I delete the parts I do not need?

Yes, and you are expected to. The file map exists for exactly this. Eight runbooks are locked against
deletion because other rules depend on them, and mobile is all-or-nothing: keep the whole set or delete
the whole set, since a half configured release path is worse than none because it looks ready.

---

## Does this replace my existing rulebook?

It migrates it. PHASE 5 moves the old rulebook across sentence by sentence and keeps a ledger of what
happened to each sentence, so a rule that was dropped was dropped visibly. A silent migration is how
teams lose the one rule that was actually load bearing.

---

## Search keeps returning the same file from the wrong application

This is a monorepo with one index at the repository root. Every query is answered from a corpus that
contains all of your applications, so the top hits are whichever files are most textually similar,
not whichever files are in the application you asked about. Citations still resolve, which is what
makes it hard to notice.

Run `<C2PY> scripts/verify/index_scope.py`. It fails if `index.placement` is `root`, if any
`index_path` sits at the repository root, if an application has no index entry, if an index is bound
to a partition it does not cover, or if `index_dir_name` is missing from `global_excludes`. The fix
is the procedure in `.agent/runbooks/monorepo-index-placement.md`: build one index per application,
inside the application, bind each one to its partition, and re-run the retrieval evaluation before
and after so the improvement is a number rather than an impression. If the repository really is one
application, say so explicitly with `placement: single_app` and `root_index_allowed: true` - the
validator accepts that, it just refuses to assume it.

---

## I only want to install the workflow, but the repo has no active goal

> Canonical detailed guide: [`C2GRAPH-WORKFLOW-ONLY-GUIDE.md`](C2GRAPH-WORKFLOW-ONLY-GUIDE.md). Copy-paste agent contract: [`C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md`](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md). Deterministic routing: [`C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md`](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md).

That state is supported. Do **not** fabricate a goal, reactivate a completed goal, or tell the agent to
run `SCAN-AND-ADOPT.prompt.md` directly. Direct SCAN selects the goal-emitting adoption path. The
workflow-only entry source must be explicit.

### Send this exact instruction to the agent

Keep the unpacked C2GRAPH distribution outside the existing, non-empty target repository. Define it as `C2GRAPH_SOURCE_ROOT`, define the project as `TARGET_ROOT`, then send one
message:

```text
Read `C2GRAPH_SOURCE_ROOT/INSTALL-WORKFLOW-ONLY.prompt.md` and execute it in full, from its first
instruction through its final STOP condition. This request is strictly `ENTRY_MODE=WORKFLOW_ONLY`.

Do not switch to `ADOPT_WITH_GOAL` or `BOOTSTRAP`. Do not execute `SCAN-AND-ADOPT.prompt.md` as a
standalone entry point; it may only be reached through the workflow-only wrapper. Do not create,
adopt, inherit, reactivate, supersede, abandon, complete, or otherwise mutate any goal. Do not modify
product/application source, business logic, migrations, product tests, or feature configuration.

Before the first write, take the goal-plane snapshot required by the wrapper. Execute all delegated
SCAN phases and all applicable gates; do not skip human decisions, verification, preservation checks,
or STOP rules. In PHASE 8 execute only the WORKFLOW_ONLY finalization branch.

Finish by reporting `ENTRY_MODE`, `GOAL_STATE_BEFORE`, `GOAL_STATE_AFTER`, `GOAL_MUTATIONS`,
`PRODUCT_CHANGES`, `WORKFLOW_STATUS`, and `PRODUCT_READY`. A successful run must have
`GOAL_MUTATIONS: 0` and `PRODUCT_CHANGES: 0`. Stop after the C2GRAPH workflow is installed, repaired,
upgraded, or verified; do not begin product work.

If the repository is empty/new, product changes are required, goal records would need mutation to make
verification pass, or the wrapper cannot be followed exactly, STOP and report the blocker plus the
correct next entry point. Do not infer or create a goal on my behalf.
```

If your host cannot read repository files and you need to paste the wrapper itself, copy the complete
file without truncating it:

```bash
# macOS
pbcopy < INSTALL-WORKFLOW-ONLY.prompt.md

# Linux/X11
xclip -selection clipboard < INSTALL-WORKFLOW-ONLY.prompt.md

# Linux/Wayland
wl-copy < INSTALL-WORKFLOW-ONLY.prompt.md
```

```powershell
# Windows PowerShell
Get-Content -Raw INSTALL-WORKFLOW-ONLY.prompt.md | Set-Clipboard
```

### What the agent must prove before the first write

The wrapper records whether `.agent/goals/goals.jsonl` exists and hashes every pre-existing regular
file below `.agent/goals/`. If `scripts/goal/gate.py` already exists, it runs without
`--require-active`; if C2GRAPH is not installed yet, the existence/hash snapshot remains the
authoritative before-state evidence until the installed gate can classify the ledger.

The permitted before-state is not limited to "no goals". The wrapper can preserve `absent`, `empty`,
`active`, `terminal_only`, or `inactive`. A malformed or contradictory ledger is classified `invalid`:
the workflow itself may still be repaired only while goal records remain untouched, and product work
stays blocked.

### What the agent is allowed to change

Only C2GRAPH control-plane artifacts, generated adoption documentation, approved tooling, and the
rulebook target explicitly allowed by the delegated SCAN path are writable. Product/application
source, business logic, migrations, product tests, feature configuration, and logical goal records are
out of scope. Creating the shipped `.agent/goals/` directory or `.gitkeep` is control-plane
instantiation; adding or rewriting a logical goal record is not.

### What success must look like

The final report must contain all seven fields, not a shortened subset:

```text
ENTRY_MODE       : WORKFLOW_ONLY
GOAL_STATE_BEFORE: absent | empty | active | terminal_only | inactive | invalid
GOAL_STATE_AFTER : absent | empty | active | terminal_only | inactive | invalid
GOAL_MUTATIONS   : 0
PRODUCT_CHANGES  : 0
WORKFLOW_STATUS  : installed | repaired | upgraded | verified
PRODUCT_READY    : no
```

For a valid run, `GOAL_STATE_AFTER` has the same meaning as `GOAL_STATE_BEFORE`; pre-existing goal file
hashes still match; `GOAL_MUTATIONS` is zero; and `PRODUCT_CHANGES` is zero. An `invalid` state is not
silently normalized by installation.

Before a later product implementation, create/select the real objective and run:

```bash
<C2PY> scripts/goal/gate.py --require-active
```

If that command fails, stop product work. Installing workflow machinery is not permission to
implement a feature.

---

## Will it change my code?

PHASE 0-2 keeps the target byte-identical: inventory, failure records, drafts, discipline rows,
discovery cache, and handoff state remain pending in session/host evidence. After PHASE 3.W, a
control-plane write is legal only when its exact destination/action is semantically required, appears
in `ACTUAL_WRITE_SET`, has path-scoped authority, and is fresh. Product source and behaviour remain
forbidden for the entire `WORKFLOW_ONLY` run.

---

## The provenance export produced zero records

The exporter reads `docs/graph-engineering/_provenance.jsonl`, which is a runtime artifact: it is
declared in `.agent/runtime-artifacts.txt` and deliberately not shipped, so a fresh install has
nothing to export and says so instead of inventing records. Run
`<C2PY> scripts/provenance/export.py --selftest` to exercise the contract against the worked
example in `.agent/provenance.schema.md`. If that passes and your own ledger still exports nothing,
the ledger is empty or every line failed to parse; the dry run prints the reason per line.

---

## The digest chain does not verify

`--verify` recomputes each record's digest and compares it to the `prevDigest` of the next one, so a
single edited record fails every record after it. That is the intended behaviour and the reason the
chain exists. Do not repair it by recomputing the chain: append a new record that supersedes the old
one, set its status to `superseded`, and leave the original in place. A ledger you can silently
rewrite proves nothing, which is also why the chain is described as integrity rather than
authorship.

---

## An accepted record has no `acceptedBy`

The check is `provenance_required_fields_enforced`, and it fails closed on purpose. A decision in
`accepted` or `superseded` state is a decision somebody took responsibility for; if no name is
attached, the state is wrong, not the check. Either name the person, or move the record back to
`proposed` or `needs_human_decision`. Setting `provenance_actor_unknown_allowed: true` is available
and is the wrong fix in almost every case: it converts a missing accountability record into a
silence.

---

## `compliance.py` says a row cites evidence that does not exist

The evidence column is a path in this repository, checked with the file system, not a description.
The usual cause is deleting an optional area and leaving its rows behind: if you removed
`scripts/interview/`, the rows that cite it are now pointing at nothing. Fix the row, do not switch
off `compliance_evidence_required`. A control map whose evidence paths are unverified is the exact
artifact this check exists to prevent, and it fails loudly on adoption day rather than quietly in
an audit.

---

## `compliance.py` says a rule cites fewer than three declared frameworks

A dash in the SOC 2, PCI DSS or EU AI Act column reads as "filled in" and counts as nothing. If a
rule genuinely maps to only some of the frameworks you care about, lower
`compliance_frameworks_declared` for the whole tree rather than hollowing out single rows, or move
the rule into the unmapped budget and say why in prose. If you are over that budget, that is the
check telling you the map has stopped being a map.

---

## `preserve.py` says a shipped rule is not in the registry

This fires when the router was regenerated instead of adjusted. It is the failure the whole layer
exists for: a scan can only see rules whose situation has already happened, so the rules that
survive because nothing has gone wrong yet are the first to be lost. Recover the identifier from
`.agent/core-baseline.md`, decide honestly where it went, and write the row. Deleting the baseline
is not a fix; `core_baseline_digest` makes that visible too.

---

## `preserve.py` refuses a rejected rule

By default `rule_rejection_max` is `0`, so a fresh install may not reject anything at all, and
`rule_rejection_requires_approver` means the approver column needs a human name. `agent`, `ai`,
`n/a` and an empty cell are all read as no approver. If a rule truly does not apply to your project,
prefer `moved` or `absorbed` with a destination that says so; rejection is for the rare case where
the obligation itself is wrong for you, and it should cost a conversation.

---

## The ledger example names `c2graph` as the acceptor

That is a placeholder in the shipped example, not a suggestion. `acceptedBy` records the human who
took responsibility for a decision, and naming the tool there produces a ledger that satisfies the
field check while answering nobody's question. Replace it with a real name on the first real record.
The same reasoning is why the approver column in the rule registry may not be a machine.



## FASE 3 says `_decisions.md` is unauthorized and asks for a snapshot

**Symptom:** preliminary preflight reports many identical collisions, often hundreds, with zero new
files; the run then says it cannot ask the next PHASE 3 question because
`docs/graph-engineering/_decisions.md` cannot be updated under `new_files_only`.

**Cause:** that combines two different concepts. Preliminary collision inventory is not final rollback
authority, and decision persistence is not a prerequisite for continuing the human interview. A count
such as `745 identical collisions` is especially suspicious when it is larger than the C2GRAPH
distribution: verify that `C2GRAPH_SOURCE_ROOT` and `TARGET_ROOT` are distinct and that preflight is not
comparing the target with itself or treating the whole target tree as the distribution.

**Correct behavior:**

1. PHASE 0 runs preliminary preflight only and records `rollback_candidate pending_write_plan` plus the
   rollback basis; it does not infer authority from identical/differing collision counts.
2. PHASE 0-2 keep answers and audit state in session/host evidence. An uncovered `_decisions.md` path
   becomes `PENDING_DECISION`, not a stop.
3. Ask every remaining PHASE 3 question through the non-writing channel.
4. After mutation-affecting answers are known, build exact `ACTUAL_WRITE_SET` and validate semantic scope.
5. Run plan-specific preflight with repeated `--plan-op ACTION:PATH`, or `--plan-empty` when no target
   byte must change.
6. Stop for rollback authority only when an **actual planned existing-file MODIFY/DELETE** lacks an
   independent before-image, or when the actual path is unsafe. The reason may be
   `WRITESET_AUTHORIZATION`; the canonical blocker class is `permission` (or `gate_red` for unsafe path
   evidence), never an eighth blocker class.
7. Optional persistence such as decisions/failures/cache/handoff remains pending unless its exact path is
   genuinely required in the actual plan. It must not widen authority merely for audit convenience.
8. Re-run the exact plan before the first write. Immediately before every individual mutation, run
   `--verify-op ACTION:PATH --expect-state absent|sha256:<hash>` using the plan's emitted expected
   pre-state. Freshness failure returns to PHASE 3.W before that path is touched.

### The snapshot candidate does not disappear after Q-ROLLBACK

**This is expected.** `snapshot_or_vcs_required` means automatic preflight has no independent shipped
before-image for those exact existing paths. After the human names an external snapshot/archive or VCS
rollback unit, bind its exact path coverage, restore owner, immutable/read-only evidence, and
`write_plan_sha256` as `ROLLBACK_AUTHORITY=proven:<named-unit>`. Do not loop waiting for automatic
preflight to return a different candidate. Re-check the named unit before every dependent write. If it
is missing/changed or coverage is ambiguous, the `permission` blocker is real.

### CREATE still fails after `new_files_only` because a parent is absent

That is also expected. Missing parents are derived filesystem actions, not implicit side effects. For
each emitted `EXPECT_DIR_ABSENT`, run `--verify-op mkdir:<path> --expect-state absent`, create exactly
that one directory top-down, then run CREATE freshness. Do not use recursive parent creation. If a
parent appears externally before its MKDIR action, return to PHASE 3.W and re-plan.

`new_files_only` describes rollback mechanics for planned CREATE operations; it does not grant semantic
scope. `distribution_restore` may cover an existing shipped path only when the source distribution is
independent and its bytes provide the exact restore image. A self/nested source cannot prove that
before-image.

See [C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md).

---

## Unsafe GATE 0 probe changed the repository

If discovery executed `npx`, a package manager, runner, project-local binary, help/version command,
or another active probe and a path such as `node_modules/.ignored` appeared, the run crossed the
pre-authority mutation barrier.

1. STOP immediately; do not continue merely because product work has not started.
2. Inventory exact changed/created paths and preserve evidence of their pre-run state when available.
3. Do not write `.agent/failures.jsonl` merely to record the incident while authority is pending;
   keep a complete `PENDING_FAILURE` in session evidence.
4. Report exact observed `TARGET_MUTATIONS`; when complete coverage is unavailable, report `unknown`; do not claim zero.
5. Obtain named snapshot/VCS cleanup authority or explicit human approval for the exact proven
   side-effect paths.
6. Revert only proven probe side effects, verify cleanup independently, then restart from static S1-S4
   GATE 0 and fresh install preflight.

See [C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md).

## A read keeps returning only part of one line

This is expected for a minified or unusually wide physical line. Do not increment only the line number. Reissue the
read with the exact emitted `resume_cursor.line` and `resume_cursor.char`. If many windows would be needed merely to
reconstruct generated/minified content, switch to a narrower search, parser, source artifact, or filter.

## A scan says `bounded` while a keyword was not found

Do not translate that into "the repository does not contain the keyword." The scanner is explicitly reporting that
it did not inspect the complete artifact under its per-file ceiling. If absence is material, narrow the candidate set
or run a purpose-built complete check with an appropriate budget.

## A path looks correct but read reports ambiguous or not found

Unicode normalization and invisible-space/quote variants are repaired only when one in-root file is unambiguous.
When several candidates remain, select the path explicitly. A typo suggestion is never silently opened.

## C2GRAPH v1.2.0 emergency authority

C2GRAPH v1.2.0 adds Bounded Emergency Authorization (BEA). BEA binds a host-mediated human approval
assertion to the existing canonical PHASE 3.W write plan, rollback evidence, target, run/session, exact actions, and
expiry. It does not create a second `ACTUAL_WRITE_SET`, does not widen semantic scope, and is not a generic break-glass
bypass. Host authentication and authoritative incident-state tracking remain outside the repository mechanism. See
`C2GRAPH-EMERGENCY-AUTHORITY-GUIDE.md` and the Indonesian companion guide.
