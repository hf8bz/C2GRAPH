# C2GRAPH

> **v1.3.2 Terminal Reconciliation release:** corrective hardening over the v1.3.1 package.
> Terminal execution truth now outranks stale continuity state, same-run terminal reactivation is rejected, and
> successor creation requires independently attested completion plus one globally unique, evidence-bound claim.
> CORE-40 remains read-only and repository-write authority is unchanged.

**Version:** `v1.3.2`
**Release date:** 3 September 2026
**Package predecessor:** `v1.3.1` Workflow Convergence Resume Hardening, 1 September 2026
**Author:** Hafizh Albanna - [LinkedIn](http://www.linkedin.com/in/hafizh-al-banna-63275410)
**Software DOI:** [`10.5281/zenodo.22259056`](https://doi.org/10.5281/zenodo.22259056)
**Release paper DOI:** [`10.5281/zenodo.22259065`](https://doi.org/10.5281/zenodo.22259065)

Bounded evidence read, bounded scanner ingestion, and the runtime-scoped/cache-aware model-adapter boundary are
included in this release. Product authority remains governed by the C2GRAPH rule, goal, evidence, and
write gates.

A template that lets a software project be worked on by agents in a way that is **measurable, traceable, and
reversible** - not merely worked on.

The template is **project-agnostic**. It assumes no language, framework, cloud, or team size. Everything
project-specific is represented by double-brace tokens - written in this document as `{{ }}` - which are filled in
once at adoption time. The full list lives in `PLACEHOLDERS.md`.

---

## The problem it solves

Agent-driven projects rarely fail because the agent was not smart enough. These five causes are far more common:

| Symptom | Root cause | Part of the template that handles it |
| --- | --- | --- |
| Rules pile up into one giant file, read on every request | no separation of invariants from procedures | three-layer `AGENTS.md` |
| The agent is confident and wrong | no evidence contract | `.agent/runbooks/evidence-contract.md` |
| Discovery returns wide, mixed results | no corpus partitions | `.agent/runbooks/graph-discovery.md`, CORE-18 |
| Large/minified/log evidence can flood a read or be confused with an empty failure | physical read lacked a shared bounded contract | `scripts/read/core.py`, `.agent/runbooks/bounded-read.md`, CORE-11/12 |
| No way to prove the project moved forward or backward | no guard metric | `prompts/01`, `docs/01` |
| Runs lose continuity when a resource runs out | no handoff reserve/resume capsule | `docs/graph-engineering/11-complexity-budget.md`, `.agent/runbooks/budget-resume.md`, CORE-24 |
| A swarm is used for work that needs one whole context | architecture chosen without criteria | `docs/graph-engineering/10-architecture-selection.md` |
| Duplicate nodes merged wrongly, every traversal inherits it | entity resolution without evidence and without a way back | `.agent/runbooks/entity-resolution.md` |
| The same lesson is relearned every week | lessons are unstructured and unqueryable | `.agent/lessons.jsonl`, `prompts/02` |
| After the CLI compacts, the agent redoes work it already finished | working state lived only in the conversation | `.agent/runbooks/context-compaction.md`, CORE-25 |
| Implementation stalls and asks what comes next after every stage | the objective was a message, not a durable contract | `.agent/runbooks/goal-contract.md`, CORE-26 |
| The report reads better than the run actually went | compliance was never observable per turn | `.agent/runbooks/workflow-discipline.md`, CORE-27 |
| The agent invents a procedure for a tool the team already uses | real usage was never captured from call sites | `.agent/runbooks/tool-usage-capture.md`, CORE-28 |
| Documentation and tests disagree, and the agent believes the prose | no ordering between honest and aspirational sources | `.agent/runbooks/test-to-workflow.md`, CORE-29 |
| An incident is urgent but cannot justify a wildcard write bypass | emergency pressure is confused with authority | `.agent/runbooks/emergency-authority.md`, bounded BEA |
| Repeated structured context wastes tokens or breaks model-specific representation assumptions | one representation is forced on every effective model | `.agent/runbooks/context-representation.md`, ACRP |
| Poisoned failure/lesson recall remains actionable during an incident | cognitive containment is confused with repository mutation | `.agent/runbooks/cognitive-emergency-authority.md`, CEA |
| A mobile release goes out with no way back | web assumptions applied to an irreversible artifact | the three mobile runbooks, CORE-31 |

---

## Three layers of context

```
LAYER 1  AGENTS.md              <= router_max_lines (220). Read on EVERY request. Invariants + routing only.
LAYER 2  .agent/scopes/*.md     Loaded when a given lane is active. Per-domain rules.
LAYER 3  .agent/runbooks/*.md   Loaded when a given procedure runs. Detailed steps.
```

The reason is bounded context, not a universal token estimate. A monolithic rulebook loads procedures that are not
relevant to every request; the router bounds always-loaded governance at `router_max_lines` (220), while scopes and
runbooks load when relevant. Token and cache-hit effects depend on tokenizer, host, and prompt layout; measure the
actual project through `docs/graph-engineering/04-context-budget.md` before making cost or cache claims.

---

## Package contents

239 files in total.

```
AGENTS.md                       generic router, CORE-01..CORE-40 + 46-row routing table
START-HERE.md                   3 entry prompts chooser
SCAN-AND-ADOPT.prompt.md        existing repo + goal-emitting adoption, PHASE 0-8
INSTALL-WORKFLOW-ONLY.prompt.md existing repo + workflow/control-plane only; goal optional, no goal emitted
BOOTSTRAP-NEW-REPO.prompt.md    new or empty repo + goal-emitting bootstrap, PHASE 0-8
PLACEHOLDERS.md                 54 tokens + meanings + examples
CHANGELOG.md                    release log, rule-ID stability policy, per-release audit results
CHECKLIST.md                    adoption checklist + durability checklist + 8 maturity levels
.gitattributes                  line-ending normalization (eol=lf)
.agent/config.yml               project profile: root, stack, mobile, partitions, exclusions, thresholds
.agent/scopes/                  10 example scopes + _TEMPLATE.md
.agent/runbooks/                40 runbooks + _TEMPLATE.md
.agent/runbooks/generated/      destination for runbooks generated from real tool usage
.agent/graph/schema/            nodes.md + edges.md + example.jsonl
.agent/lessons.jsonl            lesson corpus (empty) + lessons.schema.md
.agent/runs/                    agent execution log + runs.schema.md
.agent/state/                   handoff, cursor, and discipline record (written at runtime)
.agent/goals/                   emitted goal contracts (written at runtime)
.agent/requirements.schema.md   requirement schema (used by BOOTSTRAP, useful in old repos too)
.agent/handoff.schema.md        checkpoint schema: the sixteen sections and their meaning
.agent/goals.schema.md          goal schema: fields, states, and supersession
.agent/toolmap.schema.md        tool map schema: provenance, confidence, confirmation
docs/graph-engineering/         26 output documents + _decisions.md + references.md
docs/guides/                    per-lane verified-behaviour guides (written at runtime)
prompts/                        12 prompts: staged 00-06, 07 drill, 08 goal, 97 budget-resume, 98 handoff, 99 resume
eval/retrieval/golden.example.jsonl  example golden set, copied into golden.jsonl
eval/rubrics/_TEMPLATE.md       scoring rubric, evidence is non-compensable
scripts/scan/                   4 target-byte read-only scanners: tooling, census, plan, secret-safe redact
scripts/README.md               script contracts, language-agnostic
```

---

## The durability layer

Six capabilities, each with a runbook that says how, a document that says why, and gates that prove it happened.

### 1. Compaction handoff

When a CLI or IDE compacts the conversation, working state disappears and the agent quietly redoes work. Five
detection classes catch the signal - explicit announcement, context ratio, silent evidence such as a lost canary, a
host event, or a step counter. A checkpoint is persisted **before** anything is lost only when its exact post-3.W
state actions are in `ACTUAL_WRITE_SET`, path-scoped authorized, and fresh. Otherwise the same content stays
`PENDING_HANDOFF` in session/host evidence and TARGET_ROOT remains byte-read-only; if that external evidence cannot
survive compaction, the run stops rather than creating state without a rollback unit. A persisted next session
rehydrates from `.agent/state/HANDOFF.md`, echoes what it believes, reports any `DIVERGENCE`, and waits.

The checkpoint lands in the graph as a `Checkpoint` node with `CHECKPOINT_OF`, `RESUMES`, and `SUPERSEDES` edges, so
a resumed run is traceable to the run it continues rather than looking like a fresh one. Read
`docs/graph-engineering/12-compaction-handoff.md`; drill it with `prompts/07`.

### 2. Goal contract

Product implementation is goal-bound. When requirements are complete, `prompts/08-goal-emit.prompt.md` checks a
seven-condition readiness gate and emits a paste-ready block for the host's `/goal` command: finish condition, stage
order, stage gates, forbidden actions, and stop conditions. Every shipped Python command uses the host-resolved
`<C2PY>` launcher: POSIX uses `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`, while Windows uses
`<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd`. Never assume literal `python3`. Resolve it again after a CLI/session
restart.

Before product work, `<C2PY> scripts/goal/gate.py --require-active` must pass.

`INSTALL-WORKFLOW-ONLY.prompt.md` is the narrow exception: it installs or repairs the C2GRAPH control plane in an
existing repo while preserving `absent`, `empty`, `active`, `terminal_only`, or `inactive` goal state and emits no
goal. It cannot change product behaviour. A later objective gets its own goal through
`prompts/08-goal-emit.prompt.md`; a completed goal is never reactivated merely to install the workflow.

The readiness gate is strict on purpose. When it fails you get the missing items, not a goal - because a goal over
an incomplete requirement set converts a guess into a standing instruction.

### 3. Workflow discipline

Rules that are read are not the same as rules that are followed. A PREFLIGHT before work and a STATUS LINE after
every turn make compliance observable, seven ordering rules D1-D7 make it checkable, fifteen violation codes V01-V15
make it countable, and `discipline.jsonl` makes it reviewable. Host-caused violations are recorded as tooling gaps,
not as model errors.

### 4. Mobile: Android and iOS

One generic mobile scope is not enough for either platform, so the template ships two platform scopes
(`AND-01..AND-18`, `IOS-01..IOS-20`) plus three runbooks covering build and release, device QA against a five-slot
matrix, and store submission. The organizing constraint is irreversibility: users hold the binary, so a rollback
path is named before the release, never after.

### 5. Tool usage capture

Thirteen source classes are scanned for how this project actually invokes its tools, and runbooks are generated
**automatically** from the call sites, each step citing `path:span`. Destructive, ambiguous, low-confidence, and
duplicated-purpose entries are not generated - they become questions in the PHASE 3 queue, one question per message.
Tools installed with zero call sites are reported too.

### 6. Tests and plans become documents

Every test file is read and converted into obligations at high confidence; documents are read second and capped
lower, because a test is what the system does and a document is what someone once intended. Where they disagree,
both are recorded and a human decides. For new repos, every plan element is converted into the document format the
repository type actually uses, tracked in a conversion ledger so nothing is silently dropped and nothing is invented
to fill a table cell.

---

## Artifacts created at runtime

The fifteen files below are **not** shipped in this package, and that is correct: each one is born from measurement,
not from the template. References to them in other documents are therefore not dangling references.

The `Logical producer` column is provenance, **not write permission**. Every target write still needs semantic
scope, an exact post-3.W `ACTUAL_WRITE_SET` row, path-scoped authority, and current freshness. An artifact may
therefore remain session-only or absent for the entire run.

| File | Logical producer | Contents |
| --- | --- | --- |
| `.agent/tooling.yml` | GATE 0 inventory; optional post-3.W persistence | static discovery + approved execution evidence + claim limits |
| `.agent/draft/**` | PHASE 2/1 logical draft; normally session evidence | optional proposed configuration artifact |
| `.agent/requirements.jsonl` | PHASE 1 bootstrap evidence; optional post-3.W persistence | extracted requirements |
| `eval/retrieval/golden.jsonl` | `prompts/05-retrieval-eval.prompt.md` | filled-in copy of `golden.example.jsonl` |
| `docs/graph-engineering/_ledger.jsonl` | PHASE 5 of `SCAN-AND-ADOPT.prompt.md` | sentence ledger of the old rulebook |
| `.agent/toolmap.jsonl` | PHASE 1.13 / 1.6 | captured tool usage with provenance and confidence |
| `.agent/state/HANDOFF.md` | `.agent/runbooks/context-compaction.md` | the checkpoint, sixteen sections |
| `.agent/state/handoff.jsonl` | the same runbook | append-only checkpoint/resume/status event ledger |
| `.agent/state/CURSOR.json` | the same runbook | smallest resume pointer, always written last |
| `.agent/state/discipline.jsonl` | `.agent/runbooks/workflow-discipline.md` | one persisted line per authorized work turn; pre-authority rows stay `PENDING_DISCIPLINE` in session evidence |
| `.agent/goals/goals.jsonl` | `.agent/runbooks/goal-contract.md` | goal index with status and supersession |
| `.agent/goals/GOAL-<nnn>.md` | the same runbook | the paste-ready block, as handed over |
| `.agent/runbooks/generated/<tool>-<procedure>.md` | `.agent/runbooks/tool-usage-capture.md` | procedures generated from real call sites |
| `docs/guides/<lane>-verified-behaviour.md` | `.agent/runbooks/test-to-workflow.md` | confirmed, unprotected, contradictions, suspended |
| `docs/product/_conversion-ledger.md` | PHASE 5.9 of `BOOTSTRAP-NEW-REPO.prompt.md` | one row per converted plan element |

Shipping them as empty files would be actively harmful: an empty file that is syntactically valid passes gates and
hides the fact that the measurement was never run.

---

## Entry points

Read `START-HERE.md`. There are **3 entry prompts**; choose by repo state and intended outcome.

| Repo condition / outcome | Prompt |
| --- | --- |
| existing repo, adoption ends with an implementation goal | `SCAN-AND-ADOPT.prompt.md` |
| existing repo, install/repair C2GRAPH workflow only; no active goal required | `INSTALL-WORKFLOW-ONLY.prompt.md` |
| empty / brand new | `BOOTSTRAP-NEW-REPO.prompt.md` |

All three preserve the same scan, decision, and verification discipline. The workflow-only wrapper delegates to
SCAN-AND-ADOPT with `ENTRY_MODE=WORKFLOW_ONLY`; its PHASE 8 preserves the goal plane and emits no goal. The other
two use goal-emitting finalization after readiness passes.

## Quickstart

1. Unpack the C2GRAPH distribution **outside** the target repository. Do not bulk-copy it into
the target just to make the prompt or scripts available. Treat that unpacked directory as `C2GRAPH_SOURCE_ROOT` and
the project as `TARGET_ROOT`.
2. Give the agent exactly one entry prompt from the table above. PHASE 0 runs mutation-free
static discovery and a **preliminary rollback-basis scan** from `C2GRAPH_SOURCE_ROOT`; that scan deliberately
returns `ROLLBACK_AUTHORITY=pending_write_plan`. After PHASE 3 decisions, the agent builds exact `ACTUAL_WRITE_SET`
rows and reruns preflight with `--plan-op ACTION:PATH`, or `--plan-empty` when no target byte needs to change.
Existing identical shipped paths may use `distribution_restore`; an unrelated differing collision is irrelevant when
it is not in the plan.
3. Keep PHASE 0-2 target-byte read-only: no `AGENTS.v2.md`, failure/tooling ledger, draft, generated
docs, cache, lockfile, package-manager runner, or project-local tool execution. Persistence is path-scoped after
PHASE 3: generic authority for one path never authorizes another audit/state path, and persistence-only artifacts
may remain pending without blocking semantic installation work.
4. Let the entry prompt instantiate the template. If an existing `AGENTS.md` needs migration, the
candidate `AGENTS.v2.md` is created only after PHASE 3.W places that exact path in `ACTUAL_WRITE_SET` and its
path-scoped authority/freshness checks pass; the old file remains unchanged until the lossless swap gate passes.
5. Run `prompts/00-baseline-audit.prompt.md` (read-only).
6. Run `prompts/01-measurable-loop.prompt.md`. **Do not skip number 01.** Without metrics, every
later step is unprovable.
7. Everything else follows the order in `CHECKLIST.md`.

---

## Non-negotiable principles

1. **Instrument before optimizing.** An improvement number without a baseline is fiction.
2. **Lossless restructuring.** Moving a rule must never drop an obligation.
3. **Doubt is recorded, not guessed.** `needs_human_decision` is always cheaper than an assumption.
4. **Every change has a named rollback unit.**
5. **Hard gates are never trimmed for the sake of tidiness.**
6. **Every important output is traceable** to an objective, a plan, an artifact, a source, a
graph path, an evaluator verdict, and a bounded execution record. Where the ideas came from is recorded in
`docs/graph-engineering/references.md`.

## Glossary

- sliceSpecificity is the ratio of accepted results over results returned by one query.
- noiseComposition is the breakdown of discarded results by origin: other module, legacy,
generated, fixture, vendor.
- fallbackRate is the share of tasks that drop from the graph to partitioned text search.
- Golden set is the list of discovery questions with their correct answers, used to measure
Precision@5.
- Corpus partition is the division of the repo into regions that can be queried separately.
- Graph plane is the layer of `.agent/*.jsonl` artifacts holding objectives, runs, evidence,
and lessons.
- Ratchet is an improvement round that keeps a change only when the number improves.
- Fallback ladder is the order of discovery tools: graph, partitioned search, then direct
reading.

## Extra prompts

- `prompts/02-lessons-corpus.prompt.md` turns the lesson corpus into
`docs/graph-engineering/02-lessons.md`.
- `prompts/06-operations.prompt.md` turns operational notes into
`docs/graph-engineering/06-operations.md`.
- `prompts/07-compaction-drill.prompt.md` measures how your specific CLI or IDE behaves when it
compacts. Run it once per host, before you rely on the handoff mechanism rather than after.
- `prompts/08-goal-emit.prompt.md` runs the readiness gate and emits the paste-ready goal block.
- `prompts/98-compaction-handoff.prompt.md` is the pair you actually paste day to day: 98A writes
the checkpoint, 98B rehydrates from it.

## Reading order for the durability documents

| Document | Answers |
| --- | --- |
| `docs/graph-engineering/12-compaction-handoff.md` | what survives a compaction, what does not, and why the obvious fixes fail |
| `docs/graph-engineering/13-goal-contract.md` | why a goal is a contract rather than a plan, and why the gate is strict |
| `docs/graph-engineering/14-tool-usage-map.md` | why call sites outrank documentation as evidence of practice |
| `docs/graph-engineering/15-test-workflow-map.md` | why tests are read before documents, and what each test class does not prove |
| `docs/graph-engineering/16-mobile-platform.md` | why irreversibility, not platform syntax, is the organizing constraint |
| `docs/graph-engineering/17-workflow-discipline.md` | the gap between a rulebook and a run, and what closes it |
| `docs/graph-engineering/18-scan-automation.md` | which part of a scan a script measures, and which part breaks if a script is let near it |

Each one states its limits in the final section. A document that only lists benefits is marketing, and it teaches an
agent to overclaim in the same register.

## Bounded evidence read

Large, minified, unusually wide, or unfamiliar text files enter context through `scripts/read/core.py`. The reader
is stateless and repository-read-only; it applies line, UTF-8 byte, and physical-line character ceilings and returns
a generation-bound v2 cursor carrying `(line, char)`, exact source-byte offset, source-generation digest,
profile digest, and sequence. After three sequential windows, the default path requires a strategy review before
further blind pagination. Secret-prone paths remain on `scripts/scan/redact.py`. See
`.agent/runbooks/bounded-read.md` and `docs/graph-engineering/19-bounded-read.md`.


## Progress Liveness and Bounded Self-Audit

The control plane now distinguishes activity, diagnostic progress, and evidence-backed semantic progress.
Repeated failures, unchanged checkpoints, or a configurable soft threshold trigger early diagnosis; the
default hard boundary is eight hours (`progress_hard_seconds: 28800`). Read/reason/review audit lanes are
advisory (`canonical_write=false`), and the watchdog never creates repository-write authority. See
`.agent/runbooks/progress-liveness.md`.

## Terminal Reconciliation and successor admission - v1.3.2

A durable cursor is descriptive, not terminal truth. Before automatic resume, C2GRAPH checks the exact AgentRun;
`COMPLETED` cannot be reopened by a stale `open` cursor. A different run/task ID is not semantic progress. A terminal
successor requires `terminal_successor`, exact evidence digest, a globally unique canonical `basis_claim_id`, and an
independently verified predecessor. Claim IDs are consumable once across the handoff ledger, so changing class,
semantic delta, evidence path, model, reviewer, or session cannot mint another successor from the same claim.
Successor admission is read-only and MUST be revalidated under the existing state lock immediately before the
checkpoint append. See `.agent/runbooks/workflow-convergence.md` and `scripts/handoff/terminal_reconcile.py`.

## v1.2.0 hardening - 19 August 2026

C2GRAPH v1.3.2 preserves strict numeric validation for Progress Liveness & Bounded Self-Audit. JSON
booleans/numeric strings cannot masquerade as metrics, fractional integer counters fail closed, and
deadlines/confidence require finite values in range. Current progress evidence is **33/33 directed scenarios +
65,536/65,536 declared abstract states with 0 invariant violations**. Workflow Convergence is an additional
read-only governance layer and does not change canonical write-authority semantics.
