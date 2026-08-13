# C2GRAPH - deep dive
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

> Version scope: this deep dive describes mechanisms shipped together in the C2GRAPH `v1.1.0` Version 1 release, dated **13 August 2026**. 

> **C2GRAPH** is short for **Convert to Graph Engineering**.

[Docs start](C2GRAPH-DOCS-START-HERE.md) - [Release README](README.md) - [Deep dive](C2GRAPH-DEEP-DIVE.md) - [Reference](C2GRAPH-REFERENCE.md) - [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md) - [File map](C2GRAPH-FILE-MAP.md) - [Bounded read](C2GRAPH-BOUNDED-READ-GUIDE.md) - [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) - [Agent instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) - [Entry-mode matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md)

Blocker routing: [C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md).

The release post tells you what to run. This page explains why each mechanism is shaped the way it is,
and it is written for people looking after a large repository, where a wrong rule costs weeks rather
than minutes.

C2GRAPH is short for **convert to graph engineering**: it converts an ordinary repository into one
where the agent's plans, evidence and decisions live in a small graph instead of inside a chat window
that will be thrown away.

---

## The draft, and why the interview shrank

The expensive part of adopting a rulebook was never the writing. It was the interview. An earlier
design asked the human a long list of questions, and most of those questions had answers sitting in the
repository already: which directories are real lanes, which deploy path is actually used, which tests
exist, which tools are installed but never called.

The run now prepares a full logical draft before anyone is asked anything. Through PHASE 3 the draft
exists only as session/host evidence and target bytes remain unchanged; generic rollback authority does
not authorize an intermediate draft write. After PHASE 3.W, a draft artifact may be persisted only when
that exact destination/action is semantically required, appears in `ACTUAL_WRITE_SET`, has path-scoped
authority, and is fresh. Every value carries exactly one verdict.

| Verdict | Meaning | Becomes a question? |
| --- | --- | --- |
| `DERIVED` | proven from your repo, cited with a `path:line` or a command output | no |
| `DEFAULTED` | absent from your repo, filled from a template default, reversible | no, but it is reported |
| `NEEDS_CONFIRMATION` | genuinely undecidable from the repo alone | yes, exactly one question |

The rule that makes this work is the one attached to the third row: before a value may be marked
`NEEDS_CONFIRMATION`, the agent has to name the search that failed. Without that clause, uncertainty
becomes the cheapest possible move and every hard value gets handed back to you. With it, the count of
`NEEDS_CONFIRMATION` values measures how well the scan actually went, and it is reported as such.

On a new or empty repository the same stage runs with a lower ceiling. There is no code to prove
anything against, so `DERIVED` is unavailable by design. Values taken from planning documents are
marked `DOC_DERIVED` and capped at `doc_derived_confidence_cap`, partitions stay `PROVISIONAL`, and
writing a measured selectivity number before the code exists is forbidden outright. A confident number
invented on day one is the hardest kind of mistake to find later, because nothing about it looks wrong.

When the interview ends, the draft is promoted rather than rewritten: each `NEEDS_CONFIRMATION` is
replaced by your answer, the run asserts that no marker survives anywhere, and `.agent/draft/` is
deleted so no later session can mistake a proposal for the real configuration.

---

## One question per message

The interview now sends one question, stops, and waits.

This sounds like a formatting choice and it is not. A message containing eleven numbered questions is
skimmed, not read. What comes back is a short reply that answers the first two properly and the rest by
accident, and the questions most likely to be lost are the ones that matter most, because they are the
ones that need thought. One question fits on a phone screen, and what comes back is a decision.

The surrounding machinery exists so that one-at-a-time does not feel endless:

- The queue is built completely before the first question is sent, so its length can be stated up front
  and cannot grow while you are answering.
- Every question shows its position, like `Question 3 of 9`.
- Every answer is captured the moment it arrives. During PHASE 0-2 and whenever the exact persistence
  destinations are outside `ACTUAL_WRITE_SET`, it stays as ordered session/host `PENDING_DECISION`
  evidence. Persistence through the guarded answer path and `docs/graph-engineering/_decisions.md` is
  optional audit transport and occurs only when an exact post-PHASE-3.W `ACTUAL_WRITE_SET` action names
  a semantically required destination, path-scoped rollback/write authority covers it, and per-action
  freshness is green immediately before persistence. A session that dies at question six therefore
  seven without requiring an unauthorized target write.
- Three replies are offered on the first question: answer it, take the recommendation, or take the
  recommendations for everything that remains. The third ends the interview immediately, while still
  asking any blocker that has no default to fall back on.
- A queue of zero questions is a legitimate and good outcome. The run reports the verdict counts, says
  no confirmation was needed, and continues.
- When the host has no terminal to run the script in, it does not hang. With no TTY and no piped
  answers - or when called with `--emit` - the script prints the queue in ask-ready form and exits with
  code 3, and the agent puts those questions to you directly in the chat instead. Nothing is written,
  no answer is fabricated, and the gate stays stopped until you reply. The channel changes; the
  requirement for a human answer does not.

Two gates enforce the behaviour rather than trusting it: one checks that no draft or leftover marker
survived into the installed files, the other that every question was sent in its own message and
answered before the next one went out.

---

## Three entry modes, including a valid zero-goal installation state

C2GRAPH now separates *installing the workflow* from *authorizing product implementation*. Existing
repositories therefore have two different entry modes instead of one overloaded path:

| Repository state and intent | Entry | Goal effect | Product changes |
| --- | --- | --- | --- |
| existing repo, adoption must finish with an implementation goal | `SCAN-AND-ADOPT.prompt.md` | inherit and emit only after readiness | allowed only under the emitted/active goal |
| existing repo, install/repair/upgrade C2GRAPH itself | `INSTALL-WORKFLOW-ONLY.prompt.md` | preserve the entire goal plane; emit nothing | forbidden |
| empty or brand-new repo | `BOOTSTRAP-NEW-REPO.prompt.md` | emit only after readiness | application code remains outside bootstrap |

### Exact operator-to-agent instruction for workflow-only

> Canonical detailed guide: [`C2GRAPH-WORKFLOW-ONLY-GUIDE.md`](C2GRAPH-WORKFLOW-ONLY-GUIDE.md). Copy-paste agent contract: [`C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md`](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md). Deterministic routing: [`C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md`](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md).

When the agent can read files from the target repository, send this as **one message** from a session
with `C2GRAPH_SOURCE_ROOT` kept outside the target and `TARGET_ROOT` explicit:

```text
Read `INSTALL-WORKFLOW-ONLY.prompt.md` from `C2GRAPH_SOURCE_ROOT` and execute it in full against the separate `TARGET_ROOT`, from its first
instruction through its final STOP condition. This request is strictly `ENTRY_MODE=WORKFLOW_ONLY`.

Do not switch to `ADOPT_WITH_GOAL` or `BOOTSTRAP`. Do not execute `SCAN-AND-ADOPT.prompt.md` as a
standalone entry point; it may only be reached because the workflow-only wrapper delegates to it. Do
not create, adopt, inherit, reactivate, supersede, abandon, complete, or otherwise mutate any goal.
Do not modify product/application source, business logic, migrations, product tests, or feature
configuration.

Before the first write, take the goal-plane snapshot required by the wrapper. Then execute the complete
SCAN pipeline under `ENTRY_MODE=WORKFLOW_ONLY`; do not skip a phase, human decision, gate, verifier,
preservation check, or STOP rule. In PHASE 8 execute only the WORKFLOW_ONLY finalization branch.

Finish with all required status fields exactly as defined by the wrapper: `ENTRY_MODE`,
`GOAL_STATE_BEFORE`, `GOAL_STATE_AFTER`, `GOAL_MUTATIONS`, `PRODUCT_CHANGES`, `WORKFLOW_STATUS`, and
`PRODUCT_READY`. A successful workflow-only run must report `GOAL_MUTATIONS: 0` and
`PRODUCT_CHANGES: 0`. Stop after workflow installation, repair, upgrade, or verification; do not begin
product work.

If the repository is empty/new, the requested work requires a product change, a goal record would need
mutation to make verification pass, or the wrapper cannot be followed exactly, STOP and report the
blocking condition and the correct next entry point. Do not infer, fabricate, or reactivate a goal on
my behalf.
```

The first two sentences are not optional wording. They make the **entry source** explicit. Sending only
"run `SCAN-AND-ADOPT.prompt.md`" is a different request and selects `ADOPT_WITH_GOAL`, not
`WORKFLOW_ONLY`.

The workflow-only path is intentionally valid when the goal ledger is absent, empty, contains only
terminal goals, contains an inactive non-terminal goal, or already has a valid active goal. It first
snapshots the existing goal plane, then runs the same SCAN gates under `ENTRY_MODE=WORKFLOW_ONLY`.
PHASE 8 is the only branch that changes: no goal is created, inherited, reactivated, superseded,
abandoned, or completed. The final report must prove `GOAL_MUTATIONS: 0` and `PRODUCT_CHANGES: 0`.

This closes a lifecycle gap: a repository with no active implementation goal can receive workflow maintenance without inventing goal activity:
forcing a fake or reactivated goal just to install workflow machinery would make the goal ledger lie.
The integrity gate therefore has two explicit meanings. `<C2PY> scripts/goal/gate.py` validates the
ledger and permits a zero-active-goal state. `<C2PY> scripts/goal/gate.py --require-active` is the
separate authorization gate that must pass before later product implementation. When a new objective
arrives, the human runs `prompts/08-goal-emit.prompt.md`; workflow installation never invents that
objective on the human's behalf.

---

## Actual write plan and rollback authority

C2GRAPH separates four concepts that must not be collapsed into one another:

1. **preliminary rollback basis** - a read-only inventory in PHASE 0; it reports
   `rollback_candidate pending_write_plan` and never authorizes a target write;
2. **`ACTUAL_WRITE_SET`** - exact byte-changing `create|modify|delete` operations known only after
   mutation-affecting human decisions are resolved;
3. **path-scoped rollback authority** - the before-image/rollback mechanism for those exact paths; and
4. **semantic scope** - whether the selected entry mode is allowed to touch the path at all.

An existing repository may contain hundreds of identical or differing files that are irrelevant to the
planned operation. Those collisions are not blockers merely because they exist. PHASE 3.W runs
`scripts/install/preflight.py` against the actual write plan using repeated `--plan-op ACTION:PATH`, or
`--plan-empty` when verification requires zero target writes. A planned CREATE can use `new_files_only`;
a planned existing-file MODIFY/DELETE that is byte-identical to the independent shipped distribution may
use `distribution_restore`; only a planned customized existing-file change without an independent
before-image requires snapshot/VCS authority. Unsafe path aliasing, including symlink/path-shape problems
and hardlinked existing destinations, remains fail-closed.

Decision, failure, cache, discipline, tooling, and handoff persistence are audit transport. Their inability
to write is not itself a semantic blocker: keep complete pending session evidence unless the exact
persistence destination is genuinely required, admitted to `ACTUAL_WRITE_SET`, and covered. This is why an
unwritable `docs/graph-engineering/_decisions.md` must not prevent C2GRAPH from asking the next PHASE 3
question.

Plan authority is also freshness-scoped. Aggregate plan fingerprints are checked before the first write,
and the preflight-emitted target pre-state is checked again with `--verify-op` immediately before each
individual mutation. Restore-backed rows also recheck the independent source via
`--expect-source-state`; target freshness alone cannot prove rollback bytes still exist. That second check is required for multi-file plans because earlier authorized writes
naturally change the aggregate target fingerprint. It narrows concurrency risk but is not an OS-level
transaction lock.

See [C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md).

Derived parent directories are authorization actions, not incidental filesystem effects. Each missing
CREATE parent emits `EXPECT_DIR_ABSENT`, is verified with `--verify-op mkdir:<path> --expect-state
absent`, and is created top-down before the file CREATE. Likewise, `snapshot_or_vcs_required` is a
diagnostic rather than a retry loop: a human-named external snapshot/archive or VCS unit overlays only
its exact printed paths as `proven:<named-unit>`, and that unit is re-checked before every dependent
write. Final desired bytes are derived before the plan, so byte-identical no-ops never become fake
MODIFY rows.

---

## The goal contract

A summary describes. A goal constrains. That distinction is the whole point of the contract.

A goal is a file, `.agent/goals/GOAL-<nnn>.md`, and it is only accepted when seven conditions hold at
once. Its blocks appear in a fixed order: objective, done when, stages, guard metrics, always, never,
stop and ask when, resume, budget, status line. Each stage carries its own scope, what it produces, the
gate that closes it, and how to roll it back. It is capped at 120 lines as an operational decomposition guard. The release does not claim 120 lines is a universal readability threshold; goals that approach the cap should be checked for separable stages.

Editing a goal in place is forbidden. A goal that changes shape while the work proceeds cannot tell you
later whether the work succeeded, so a new goal supersedes the old one and the supersession is
recorded. Product implementation is allowed only when the projected ledger has an active goal and
`goal/gate.py --require-active` passes. A goal-bound run mirrors that goal into `goals.jsonl` and the
handoff, and represents it as a `Goal` node so later work can be traced to what it was supposed to
serve. A workflow-only installation is the deliberate exception: zero active goals is valid because
that run is forbidden from changing product behaviour or mutating goal records.

---

## Compaction handoff, and why backups come first

Every long session eventually hits a context reset, and the default failure is silent: the agent keeps
working from a confident summary of things it no longer remembers correctly.

The template treats the reset as a scheduled event. Context-pressure thresholds trigger a **logical**
checkpoint, and the workflow also refreshes continuity at its configured step cadence. Continuity is
always captured in session/transcript evidence as `C2GRAPH_RESUME_CAPSULE v1`. The three durable
target artifacts--`HANDOFF.md`, `handoff.jsonl`, and `CURSOR.json`--are persisted only when their exact
post-PHASE-3.W actions are semantically required, path-scoped authorized, and fresh. When persisted,
`CURSOR.json` is written last so a crash mid-write cannot point at work that was not recorded.

The handoff operation is capped at eight tool calls because continuity must fit inside the reserved
resource budget. If the budget/context/rate/session boundary is reached, the outcome is
`suspended_resumable`, not terminal failure. Prompt 97 opens a new resource epoch, resets local
`BUDGET_USED` to zero, and resumes exact `NEXT_ACTION`. Re-running completed discovery after resume is
forbidden.

Backups sit at the same level of seriousness. Invariant two is back up first, and reversibility is its
own invariant: no destructive step runs without a stated way back. Files touched are recorded in a
table with their change type and whether the change was committed and verified, so a rollback has
something to work from rather than a memory of what happened.

---

## Why it costs less to run

A monolithic rulebook can force unrelated procedures into the always-loaded instruction surface. The split is three layers: `AGENTS.md` is the router and is bounded by `router_max_lines` (195); scopes load when a lane is active; runbooks load when a procedure actually runs. This release does not assign a universal token or cache-hit reduction to that split because tokenizer, host caching policy, and task routing change the measured result. Measure the actual project with the context-budget procedure before reporting savings.

One evidence rule governs how this may be described. File-byte and line geometry can be counted directly; token
savings require a declared tokenizer and the actual prompt construction. Cache hit rate, answer quality, and rule
compliance are empirical and need instrumentation. These evidence classes must not be substituted for one another.

---

## Workflow discipline

Rules that are never checked decay. The discipline layer makes obedience observable.

Every run opens with a preflight line naming its phase, scope, runbook, partition and mode, and reports
a status line carrying stage, gate, units done, evidence and budget. Twelve violation types are
enumerated and logged to `.agent/state/discipline.jsonl`, and when the same violation repeats three
times the run stops rather than continuing to drift. The point is not punishment. A recurring violation
is evidence about the rule itself: either the rule is wrong for this repository, or the work is
structured in a way that keeps colliding with it.

The ratchet works the same way. A fix is only finished when the thing that allowed the defect is
closed, and a maximum of three rounds keeps that from becoming its own project.

---

## What a script does, and what still needs an agent

The early phases were entirely hand-run: the agent listed files, counted lines, and read manifests one
tool call at a time. Measurement settled the question. The current census was remeasured on deterministic synthetic repositories using three sequential warm-cache rounds on one local container. Median elapsed time was 1.362 s at 1,014 files, 2.458 s at 10,014 files, and 6.944 s at 50,014 files; all nine invocations returned exit 0 and reported the expected file count. These measurements do not support the earlier fixed 10,000-file threshold or a universal growth model. The census accepts a partition budget so the operator can bound work according to the selected target and host constraints rather than relying on a repository-size constant.

So the package now ships four target-read-only scanners. Three are general discovery scanners:
`scan/tooling.py`, `scan/census.py`, and `scan/plan.py`; the fourth, `scan/redact.py`, is the structural
reader for secret-prone files. An existing repository has code, so `scan/census.py` enumerates structure,
stack, generated files, tests, CI, rulebooks, sensitive surfaces and static Git metadata. A new repository
has no code at all, so `scan/plan.py` inventories planning artifacts instead. It emits candidate spans and
counts, never raw source-line text; secret-prone paths are deferred to `scan/redact.py`, and opaque binary
documents remain deferred until conversion. Running the wrong one is a documented mistake: pointed at a mature repository,
`plan.py` reports 189 "obligations" that are mostly noise. `scan/tooling.py` is the one script that
applies to both kinds, because a machine has the same tools either way.

What stayed with the agent is everything that requires judgement, and it stayed on purpose. A scanner
never decides which candidate sentence is a requirement, never invents an acceptance criterion, never
uses static S1-S4 evidence without executing a candidate tool, and never writes `absent` merely because a runner was not executed on
`PATH`. Each scanner ends by printing what it refused to decide - 21 items for the census, 9 for the
plan scan, 5 for the static tool inventory - so the deferral is visible rather than silent. No human gate moved.

---

## Tool practice, documented rather than assumed

The scan reads which tools exist, and also where they are actually called. A tool installed with zero
call sites is reported as exactly that, not as a capability. Two deploy paths pointing at the same
outcome become one question with evidence attached, such as which of the two ran in the last ninety
days.

What the agent may claim is bounded by what its tools can prove. If nothing on the machine can run your
tests, the run may not report that tests pass; it reports that it cannot know. Most confident wrong
answers come from this gap, and closing it is cheaper than catching them afterwards.

---

## Provenance, and the difference between integrity and authorship

The template already recorded decisions. What it did not record was who proposed them and who
accepted them, and that gap is exactly the one an auditor opens with. Three fields close it -
`model`, `provider`, `acceptedBy` - and one exporter turns the ledger into W3C PROV-O so the record
leaves the repository in a vocabulary someone else's tooling already reads.

The design decision worth naming is what was deliberately not built. Each record carries the digest
of the previous one, which makes tampering visible after the fact. It does not make tampering
impossible, and it says nothing about identity: whoever can write the ledger can rewrite the whole
chain, and `--sign` emits a SHA-256 manifest, which is the same kind of artifact a release checksum
is. Key-based signing needs key custody, and key custody in a template that ships no runtime and no
dependency would be a promise the template cannot keep. So the manifest is described as a checksum,
the chain is described as integrity, and the word signature is not used for either.

The part that does hold: an accepted decision with no named human fails the provenance check, and the
exporter refuses to export it. That is a small guarantee, stated at its real size.

## Configuration is the only place a number lives

The package enforces a lot of numbers: how long the router may get, how selective a search has to be
before its result may be used, how many questions a queue may hold, how often a checkpoint is written,
how many subagents may run at once. All of them sit in `.agent/config.yml`, and the surrounding
documents are required to name the setting rather than restate its value.

This is what makes the template tunable without a rewrite. A large repository that wants a stricter
selectivity floor changes one line, and the discovery ladder, the adoption gate and the checklist all
inherit it. Nothing needs to be hunted down, because nothing was copied.

The rule also has an audit behind it. Every threshold in the configuration is checked for at least one
reference outside the file, so a setting nobody reads shows up as dead configuration rather than
sitting there looking authoritative. And two ceilings that had been written as plain prose numbers,
the question queue and the seed interview, became real settings once the audit found them, because a
number that no key owns is a number nobody can change safely.

## An index that spans applications is noise, not coverage

A code index is the cheapest way to answer where something lives, and in a monorepo it is also the
cheapest way to answer it wrongly. One index built at the repository root indexes four applications
at once, so a query for a handler name returns the handler in each of them. Nothing errors. The agent
picks one, cites a real file and a real line, and the citation is verifiable and wrong.

The package treats that as a structural defect rather than a search-quality complaint. CORE-34
requires one index per application in a multi-application repository, placed inside the application
directory and bound to the partition it indexes, so the corpus a query searches is the same corpus
the claim is about. `.agent/config.yml` declares it as data - `placement`, `root_index_allowed`,
`index_dir_name`, and one `per_partition` entry per application - and `scripts/verify/index_scope.py`
refuses the arrangement if an index path sits at the root, if an application has no index, if an
index answers for a partition it does not cover, or if the index directory is missing from
`global_excludes`, which is what keeps generated index files from being read back as source.

The detection sits earlier than the rule. The adoption scan records `repo_shape` from named signals
with a proving path, inventories index artifacts that already exist, treats a pre-existing root index
as a finding to migrate, and asks about placement as a blocking question rather than choosing for
you. A single-application repository keeps a single index, but it has to say so.

## Mobile work is treated as irreversible

Release metadata and artifact status must remain internally consistent. Mobile scopes and their configuration block are therefore
all-or-nothing: either the whole set is present and complete, or it is deleted as a set. A half
configured release path is worse than none, because it looks ready.

## Mapping rules to controls without claiming compliance

The question that follows a provenance ledger is always the same: which control does this satisfy.
`.agent/compliance-map.md` answers it for every core rule and every evidence producing gate, in
SOC 2, PCI DSS 4.0 and EU AI Act terms, with the file that would be handed to an assessor named in
the row.

The interesting part is the line the checker refuses to cross. It verifies that the rule exists,
that the control identifier has the shape the framework publishes, that the evidence file is
present, that the table has not been thinned below `compliance_rows_min`, and that the sentence
saying this is not a certification is still there. It does not verify that the mapping is correct,
because correctness here is an assessor's judgement and a script that graded it would be producing
exactly the unfounded claim the invariants forbid.

Two rules stay deliberately unmapped. Prompt injection and secret handling cut across so many
controls that a single citation would mislead more than an honest blank, so they spend the
`compliance_unmapped_core_allowed` budget and are discussed in prose instead. A map with a gap you
can explain is worth more than a map with no gaps and one invented citation.

## Adoption day, and the rules a scan cannot see

The most expensive defect found in these rounds was not in a script. When an agent adopts a
repository that already has its own rulebook, the natural move is to regenerate the router from what
the scan found. What a scan can find is evidence: the frameworks in use, the test command, the
migration directory. What it cannot find is a rule whose situation has not happened yet - the named
rollback unit, the guard metric with a baseline, the handoff written before compaction. Those rules
survive precisely because nothing has gone wrong, and they are the first to disappear in a rewrite.
The loss is invisible, because the new router looks tidy and every rule in it is true.

So the baseline is frozen and hashed before the first adjustment, every shipped rule gets a row
saying where it lives now, and a rule may only leave the router by being relocated to a file that
exists and mentions it, or rejected with a reason and a named human approver. Identifiers are never
renumbered, not even to close the gap a rejection leaves, because a gap is information.

State the guarantee at its real size: this makes a lost rule detectable, not impossible. It cannot
tell you whether a rule that is still present is being followed, and the shipped mutation/isolation regression tests do
not prove there is no twenty-sixth way through.


For the byte-level pre-authority safety model, see [C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md).


## Secret containment and the PHASE 3 boundary

CORE-33 separates immediate containment from global stopping. Secret-prone files are structurally
inspected through `scripts/scan/redact.py`; if a raw value nevertheless reaches context in PHASE 0-2,
the affected slice is quarantined and a sanitized `PENDING_SECURITY_INCIDENT` is carried forward while
independent safe read-only discovery continues. PHASE 3 is the mandatory human security decision. This
prevents both credential propagation and discovery-phase denial of service. The complete state machine
is in [C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md).
## Cross-platform CLI and resource continuity

C2GRAPH deliberately separates the **logical Python CLI** from the executable name provided by a host. Every shipped command uses the `C2PY` abstraction, resolved through `scripts/c2python.sh` on POSIX or `scripts/c2python.cmd` on Windows. This prevents Windows exit 9009 and avoids turning GATE 0 into a package-manager/bootstrap probe. `scan/tooling.py` records the interpreter already executing it as `python-runtime`; it does not demand a binary literally named `python3`.

The same separation exists for run lifetime. A tool-call/context/rate/host limit is a scheduling boundary, not evidence that the task failed. SCAN reserves 12 calls from its 300-call cap; BOOTSTRAP reserves 12 from 200. At the soft boundary the run stops starting new work, emits a portable resume capsule, and returns `suspended_resumable`. PHASE/FASE 3/3.W always emits or refreshes that capsule before PHASE/FASE 4, so an unexpected CLI closure after decisions are settled has a continuation anchor even when no target-state file was authorized.

Resume through Prompt 97 re-resolves `C2PY`, validates/echoes the newest capsule, opens a new execution/resource epoch (`BUDGET_EPOCH + 1`, local `BUDGET_USED = 0`, cumulative usage audit-only), performs only divergence/freshness required by the exact next action, and continues. It does not replay completed discovery or human decisions. If the hard resource cap has already been reached, capsule emission is text-only from evidence already present in context; no extra tool call is required merely to checkpoint.


## The read boundary is now engineered

C2GRAPH v1.1.0 constrains discovery semantically and also applies a physical ingestion boundary. Unknown/large text
is converted into bounded evidence under independent line, UTF-8 byte, and per-physical-line ceilings. Human coordinates remain `(line, char)`, but emitted Bounded Read v2 continuations bind exact source-byte offset,
source generation, ceiling profile, sequence, and previous-view digest. Generated continuations seek directly from
the recorded offset, and the default fourth blind sequential window requires strategy review. Typed outcomes
distinguish empty, failed/unsafe, stale-generation, profile-mismatch, and strategy-review states. Scanner integration
preserves the same rule: an incomplete bounded scan cannot prove global absence. Read receipts remain ephemeral and
do not become canonical repository state.

## Host model adapter as a non-authoritative compatibility layer

C2GRAPH does not parse every provider protocol in its authority core. A host-side adapter normalizes structured tool calls, native identifiers, runtime parser capabilities and cache hints into a provider-neutral capability document. The C2GRAPH admission validator can fail closed on missing live/runtime/cache evidence, but it does not promote private reasoning, raw prompt prefixes or cache-routing keys into durable state. This keeps protocol compatibility and cache optimization orthogonal to goal/write authority.
