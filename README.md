# C2GRAPH

**Bahasa Indonesia:** [README-ID.md](README-ID.md)

### Convert to Graph Engineering

**Repository-level governance for long-running AI coding agents.**

C2GRAPH makes agent-driven software work more **bounded, measurable, traceable, resumable, reversible, and auditable** by moving critical execution state out of transient conversation history and into explicit repository contracts.

[![Release](https://img.shields.io/badge/release-v1.3.2-blue)](https://github.com/hf8bz/C2GRAPH/releases/tag/v.1.3.2)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Paper DOI](https://img.shields.io/badge/Paper_DOI-10.5281%2Fzenodo.22259065-blue)](https://doi.org/10.5281/zenodo.22259065)
[![Software DOI](https://img.shields.io/badge/Software_DOI-10.5281%2Fzenodo.22259056-blue)](https://doi.org/10.5281/zenodo.22259056)
[![Source checks](https://img.shields.io/badge/source_checks-43%2F43_EN_%7C_43%2F43_ID-brightgreen)](https://github.com/hf8bz/C2GRAPH/releases/tag/v.1.3.2)

**Version 1.3.2 · Public release · 3 September 2026**

**Previous public release:** v1.2.0 · 22 August 2026  
**Technical predecessor:** v1.3.1 · 1 September 2026

[Release Notes](https://github.com/hf8bz/C2GRAPH/releases/tag/v.1.3.2) ·
[Release Paper](https://doi.org/10.5281/zenodo.22259065) ·
[Software Archive](https://doi.org/10.5281/zenodo.22259056) ·
[Docs Index](C2GRAPH-PUBLIC-DOCS-INDEX.md) ·
[Release Overview](C2GRAPH-V1.3.2-RELEASE-OVERVIEW.md) ·
[Terminal Reconciliation](C2GRAPH-TERMINAL-RECONCILIATION-GUIDE.md) ·
[Bahasa Indonesia](README-ID.md)

---

## Why C2GRAPH exists

Modern coding agents can write code well.

The harder problem begins when the work becomes long-running.

An agent can still:

* lose its objective after context compaction;
* rediscover the same repository facts repeatedly;
* mix evidence from different applications in a monorepo;
* retry an approach that already failed;
* convert an inference into an implementation requirement;
* call tools through the wrong model/runtime protocol;
* widen mutation scope because another change looks useful;
* lose rollback context;
* or report completion with evidence that does not actually prove the claim.

C2GRAPH treats many of these problems not as mysterious model failures, but as **governance and state-transition problems that can be made explicit**.

The central principle is:

> **Knowledge does not automatically become authority.**

A search result is not permission to write.

A Goal is not arbitrary write authority.

A cache hit is not evidence of correctness.

A valid tool call does not prove that the model chose the right tool.

A rollback mechanism does not widen semantic scope.

A checkpoint does not create permission to mutate repository state.

And confidence is not verification.

---

# What is Graph Engineering?

**C2GRAPH means Convert to Graph Engineering.**

Graph Engineering is not simply adding a graph database to an AI system.

It means turning important relationships in engineering work into explicit, inspectable state.

For example:

```text
Objective
   │
   ▼
 Goal ───────────────► Requirements
   │                       │
   ▼                       ▼
 Task ─────► Run ─────► Artifact
   │          │             │
   │          ▼             ▼
   │       Evidence ─────► Claim
   │          │             │
   ▼          ▼             ▼
Decision    Metric       Evaluation
   │
   ▼
Mutation ──► Verification
   │
   ├────────► Failure ──► Lesson
   │
   └────────► Checkpoint ──► Resume
```

The objective is not to put everything into a graph.

The objective is to make enough of the work explicit that another session—or another person—can answer:

* What are we trying to accomplish?
* What has actually been done?
* What evidence supports it?
* What remains ambiguous?
* What is allowed to change next?
* What can restore the previous state?
* What failed?
* What did we learn?
* Where should execution resume?
* What evidence justifies saying the work is complete?

---

# Core design principles

C2GRAPH can be summarized through four principles.

### 1. Bounded Autonomy

Agents can work autonomously, but expansion is bounded across context, retrieval, graph traversal, mutation scope, retry loops, batching, concurrency, and resource use.

### 2. Evidence-Bound Authority

Important actions are tied to explicit evidence, scope, provenance, rollback, freshness, and verification rather than model confidence alone.

### 3. Durable Operational Learning

Failures, corrections, lessons, discoveries, decisions, and continuation state can survive session boundaries without becoming permanent unquestioned truth.

### 4. Compatibility Without Authority Leakage

Provider-specific model behavior, tool-call formats, reasoning state, and cache controls remain at the host/runtime adapter boundary rather than silently becoming repository authority.

---

# What changed from v1.2.0 to v1.3.2

v1.3.2 is a backward-compatible Version 1 governance and corrective-hardening release over the previous public release, v1.2.0. It preserves the canonical mutation predicate: exact semantic scope, `ACTUAL_WRITE_SET`, rollback readiness, fresh pre-state, and effect-time validation remain the basis for repository mutation.

The 1.3.x line adds a convergence and terminal-state boundary around the v1.2.0 authority model. It does **not** create broader repository-write authority.

### CORE-40 Workflow Convergence

C2GRAPH now distinguishes **semantic progress** from procedural activity. Recovery against an unchanged semantic frontier is bounded:

```text
NO_PROGRESS_OBSERVED
        │
        ▼
DIAGNOSE_ONCE
        │
        ▼
still no semantic delta?
        │
        ▼
STOP_BUSY_LIVELOCK
```

Changing a run ID, task ID, model, reviewer, session, wording, or audit pass is not semantic progress by itself. Re-planning is accepted only through the configured closed invalidator set.

### Sticky completion and PASS-only verification reuse

Completion is sticky across resume/recovery paths. A stale or partial continuity surface cannot silently reopen a completed frontier. Verification reuse is bound to an exact verification identity and prior `PASS`; changed identity, contradictory state, or unsupported status requires fresh verification or fails closed.

### Cross-plane terminal reconciliation

v1.3.2 introduces a terminal boundary across durable execution and continuity state.

If a unique AgentRun is already `COMPLETED`, stale `open` CURSOR/HANDOFF state cannot reactivate the same run. Execution-plane terminal truth outranks stale descriptive continuity for same-run resume.

When a completed predecessor carries an `evaluation_id`, terminal verification requires exactly one same-run evaluation with verdict `kept`. Missing, duplicate, wrong-run, contradictory, or non-`kept` evaluation state fails closed.

### Evidence-bound terminal successor admission

A different run ID is not progress by itself. A successor after terminal completion requires explicit `terminal_successor` metadata that binds the predecessor, allowed successor class, meaningful semantic delta, evidence path, exact SHA-256, and canonical `basis_claim_id`.

The claim identity is globally consumable once by semantic identity. Changing class, delta, run/task ID, session, model, reviewer, evidence path, or copying the same evidence does not mint another successor.

### Evidence-path and alias hardening

Terminal-successor basis evidence must be a root-contained regular UTF-8 file with an exact digest and exact-token claim occurrence. Direct or symlink-aliased continuity/audit/control-plane paths such as `.agent/state/**`, `.agent/runs/**`, `.agent/graph/**`, and `backups/**` are rejected as successor basis.

### Resume mode and successor mode are separate

`handoff/terminal_reconcile.py` now exposes two distinct questions:

```text
Can this existing run resume?
```

and:

```text
Can this verified terminal predecessor admit a new successor?
```

Default mode is the resume gate. `--successor-json FILE` is successor admission. A correctly terminal predecessor is therefore expected to fail same-run resume while still being eligible to admit a valid, independently evidenced successor.

### Lock/fence revalidation before append

The terminal admission helper is read-only. A successful preflight is not durable write permission. Writers must use the existing state lock/fence and revalidate predecessor/claim state immediately before append, narrowing the time-of-check/time-of-use window without creating a new write-authority primitive.

### v1.2.0 authority capabilities remain intact

The following v1.2.0 additions remain part of v1.3.2:

* **Bounded Emergency Authorization (BEA v2)** — a short-lived, human-granted overlay for an already materialized exact write plan; never wildcard authority.
* **Adaptive Context Representation Planning (ACRP)** — changes representation of an already selected/frozen evidence set without changing semantic membership or authority.
* **C2GRAPH-native Cognitive Emergency Authorization (CEA)** — bounded containment for exact recalled failure/lesson objects without turning cognition into repository-write authority.
* **Progress Liveness & Bounded Self-Audit** — distinguishes semantic progress, benign wait, long-running work, inconsistency, and stagnation; recovery remains bounded and non-authoritative.
* **Handoff telemetry integrity** — current checkpoint rows remain immutable; resume telemetry is separate and subject to normal write authority.
* **Authority non-interference** — routing/cache state, ACRP, CEA, incident severity, liveness, convergence, and terminal reconciliation may influence strategy or admission, but do not widen the canonical exact-write set.

---

# How C2GRAPH is structured

## Three-layer context architecture

C2GRAPH avoids placing every instruction into one permanently loaded rulebook.

```text
Layer 1
AGENTS.md
Core invariants + routing
        │
        ▼
Layer 2
Domain scopes
Loaded when the relevant lane is active
        │
        ▼
Layer 3
Runbooks
Loaded when a concrete procedure is executed
```

This makes context consumption explicit and helps keep stable instructions stable while loading detailed procedures only when needed.

It is a **cache-friendly architecture**, not a guarantee that any particular model provider will produce a cache hit.

---

# Entry modes

C2GRAPH supports three repository conditions.

| Mode              | Use when                                                                        | Product mutation                                            |
| ----------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `ADOPT_WITH_GOAL` | Existing repository that will proceed into implementation work                  | Allowed only after Goal admission and exact write authority |
| `WORKFLOW_ONLY`   | Install, repair, or verify C2GRAPH governance without changing product behavior | No                                                          |
| `BOOTSTRAP`       | New or empty repository                                                         | Gated after bootstrap planning                              |

A missing Goal is therefore **not automatically an error**.

`WORKFLOW_ONLY` deliberately supports zero-goal governance work.

Start with:

Operational entry contract: `START-HERE.md` plus the three shipped entry prompts.

---

# Key capabilities

## Goal Contract

A Goal is durable execution state rather than a free-form prompt.

Goal readiness can account for:

* requirements;
* acceptance criteria;
* ambiguity;
* conflicts;
* stage ordering;
* human decisions;
* guard metrics;
* rollback units;
* resource/complexity budgets;
* forbidden actions;
* finish conditions;
* lifecycle and supersession.

An admitted Goal defines what the work is trying to accomplish.

It still does **not** authorize arbitrary repository writes.

---

## Exact Write Authority

Before mutation, C2GRAPH converges on an exact:

```text
ACTUAL_WRITE_SET
├── CREATE
├── MODIFY
└── DELETE
```

No-op candidates are removed before authorization.

Every real mutation must satisfy independent conditions around:

```text
semantic scope
      ↓
exact action + path
      ↓
rollback authority
      ↓
fresh expected pre-state
      ↓
mutation
```

If execution discovers another required mutation later, that mutation does not silently inherit authority.

It returns to planning.

See:

Operational details are indexed in the [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md), including Goal, incident rollback, and rule-preservation procedures.

---

## Transaction-specific rollback

C2GRAPH does not reduce rollback to “make a backup.”

Different mutations need different recovery strategies.

Examples include:

* CREATE-only rollback;
* independent distribution before-image;
* VCS or external snapshot for customized files;
* pre-state and freshness validation before dependent writes.

The governing question is:

> **Can this exact mutation be reversed using a valid recovery basis independent from the mutation itself?**

Rollback capability also does not grant semantic permission to make the change.

---

## Mutation-free GATE 0

Repository discovery begins conservatively.

C2GRAPH distinguishes:

**static discovery**

from

**active execution**.

The presence of a tool or a command such as `--help` or `--version` is not automatically treated as proof that executing it is side-effect free.

Project tooling, package managers, runners, network actions, hooks, generators, and other execution surfaces remain gated until their use is justified.

---

## Secret-aware discovery

Repository scanning should not unnecessarily inject secrets into model context.

C2GRAPH separates:

* secret-prone file detection;
* structural discovery;
* bounded/redacted inspection;
* accidental exposure;
* incident handling;
* continuation decisions.

The objective is to discover repository structure while minimizing unnecessary secret-value admission.

See:

Operational details are indexed in the [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md), including untrusted-content and evidence-contract procedures.

---

# Bounded Evidence Read v2

Reading is itself governed.

Bounded Evidence Read v2 provides bounded physical evidence admission with controls for:

* physical line count;
* normalized evidence bytes;
* per-line character size;
* path containment;
* generation-bound continuation;
* opaque cursors;
* stale-cursor detection;
* bounded JSONL handling;
* bounded archive inspection;
* Unicode-safe handling;
* freshness;
* strategy review after repeated blind pagination.

```text
large artifact
      │
      ▼
bounded evidence window
      │
      ├── enough evidence? ──► continue workflow
      │
      └── not enough?
              │
              ▼
       cursor / targeted search
       range / parser / tail
       or strategy change
```

Bounded Read is not intended to become endless bounded scrolling.

When sequential windows stop being the right retrieval strategy, the workflow can switch strategy.

A successful read also does not bypass Goal, evidence, freshness, or write-authority gates.

See:

Operational bounded-read contracts are indexed in the [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md) and [Script & Verification Catalog](C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md).

---

# Partition-first discovery

Repository-wide search can be dangerous in a large monorepo.

Two applications can contain identical:

* class names;
* routes;
* DTOs;
* configuration names;
* database concepts;
* directory structures.

A technically correct match from the wrong application is still the wrong evidence.

C2GRAPH therefore supports explicit corpus/application partitions.

The conceptual fallback ladder is:

```text
Graph
  ↓
Partitioned Search
  ↓
Direct Bounded Evidence Read
```

If search outperforms the graph for an exact question, that is accepted.

The graph is a retrieval mechanism—not an ideology and not authority.

---

# Per-application index boundaries

A single repository-wide index is not automatically desirable.

For multi-application repositories, C2GRAPH can scope indexes to an application or justified partition to reduce cross-product contamination.

The index remains disposable and rebuildable.

It does not become execution authority.

---

# Typed Graph Plane

C2GRAPH separates **work lineage** from **domain knowledge**.

### Work lineage

Tracks relationships among concepts such as:

* Objective;
* Goal;
* Task;
* Run;
* Artifact;
* Commit;
* Evaluation;
* Metric;
* Checkpoint.

### Domain knowledge

Tracks concepts such as:

* Source;
* Claim;
* relation;
* contradiction;
* evidence;
* decision;
* lesson.

C2GRAPH also separates operational concerns into:

```text
Control
Execution
Artifact
Graph
Evaluation
```

This avoids turning one conversation transcript into the workflow engine, database, memory store, and audit log at the same time.

---

# Progressive query architecture

C2GRAPH does **not** require a graph database.

The query layer can grow only when actual query complexity justifies it:

```text
Repository JSONL
      ↓
jq / bounded filters
      ↓
SQLite / DuckDB
      ↓
Graph database
only if deeper traversal earns the complexity
```

Higher-level indexes are derived infrastructure.

They are not the source of execution authority.

---

# Failure Memory

A failure should survive longer than the conversation in which it happened.

Failure state can include:

* what failed;
* failure class;
* blocked action;
* observed signature;
* known cause;
* wrong approach;
* `correct_approach`;
* evidence;
* retry policy;
* lifecycle.

Before repeating a relevant open failure, the workflow can ask:

> **What materially changed since this failed last time?**

This reduces failure loops caused purely by session amnesia.

---

# Durable Lessons

Resolved failures and human corrections can become reusable lessons.

Lessons retain provenance and ageing semantics.

That allows knowledge to survive sessions while still distinguishing:

* current evidence;
* confirmed lessons;
* stale knowledge;
* unconfirmed assumptions.

This is **repository-level operational learning**.

It is not a claim that the underlying LLM retrains itself.

---

# Workflow discipline

Reading a rulebook does not prove that the rulebook is being followed.

C2GRAPH makes workflow discipline observable through:

* preflight state;
* per-turn status;
* sequencing rules;
* violation classes;
* violation ledger;
* escalation;
* session-start recall;
* tooling-gap classification.

Examples of observable failure modes include:

* skipping required phases;
* claiming without evidence;
* continuing after a STOP condition;
* scope creep;
* guessing a human answer;
* resuming without rehydration;
* repeating work unnecessarily;
* retrying a known failure without a material change.

Repeated procedural violations can stop a run.

---

# Progress Liveness & Bounded Self-Audit

Visible activity is not necessarily semantic progress.

C2GRAPH can classify current execution as:

* semantic progress;
* benign external wait;
* legitimate long-running work;
* accounting inconsistency;
* stagnation.

The liveness surface is diagnostic. It cannot manufacture `ACTUAL_WRITE_SET`, bypass Goal authority, or convert a diagnosis into mutation permission.

If stagnation is detected, bounded recovery feeds into CORE-40 Workflow Convergence rather than starting an unlimited self-audit loop.

See:

**[Workflow Convergence Guide](C2GRAPH-WORKFLOW-CONVERGENCE-GUIDE.md)**

---

# Workflow Convergence

Progress detection and recovery termination are separate responsibilities.

C2GRAPH represents a semantic frontier across goal revision, task, next action, evidence, decision, actual write set, blockers, gate state, and completion state. Repeated activity against an unchanged frontier does not count as progress.

```text
unchanged frontier
        │
        ▼
NO_PROGRESS_OBSERVED
        │
        ▼
DIAGNOSE_ONCE
        │
        ▼
still unchanged
        │
        ▼
STOP_BUSY_LIVELOCK
```

A closed invalidator can legally reset the convergence budget. External wait remains evidence-bound. Completion remains sticky. Verification reuse remains PASS-only and exact-identity bound.

Workflow Convergence is non-authoritative: it determines whether recovery should continue, wait, replan, close, or stop. It does not create repository-write authority.

---

# Terminal Reconciliation and successor admission

Convergence answers whether reasoning/recovery should continue. Terminal reconciliation answers whether durable continuity may treat a run as resumable.

For same-run resume:

```text
CURSOR / HANDOFF
        │
        ▼
exact run_id
        │
        ▼
AgentRun plane
        │
        ├── ACTIVE / resumable state ──► continue normal validation
        │
        └── COMPLETED
                │
                ▼
        same-run resume rejected
```

If the completed run has `evaluation_id`, exactly one matching same-run `kept` evaluation is required for independently verified completion.

A new successor requires more than a new identifier:

```text
verified terminal predecessor
        │
        ▼
terminal_successor
        │
        ├── predecessor_run_id
        ├── predecessor_evaluation_id when required
        ├── closed successor class
        ├── meaningful semantic_delta
        ├── basis_ref
        ├── basis_sha256
        └── basis_claim_id
                │
                ▼
claim not previously consumed?
        │
        ▼
lock / fence + immediate revalidation
        │
        ▼
append successor handoff state
```

Key fail-closed properties:

* a stale `open` cursor cannot reactivate a completed run;
* a non-`kept` explicit evaluation cannot be overridden by weaker completion evidence;
* duplicate/malformed run or evaluation planes fail closed;
* predecessor evaluation binding must be exact when present;
* `basis_claim_id` must be canonical and globally unique in the handoff ledger;
* token-prefix tricks such as `CLAIM-1` matching `CLAIM-10` are rejected;
* proposal reshaping cannot reuse a consumed semantic claim;
* copied evidence does not create a new claim;
* direct or symlink-aliased control-plane evidence is rejected;
* admission remains read-only and does not widen write authority.

See:

**[Terminal Reconciliation Guide](C2GRAPH-TERMINAL-RECONCILIATION-GUIDE.md)**

---

# Ratchet loops

C2GRAPH supports bounded iterative optimization only when the result can actually be measured.

The basic loop is:

```text
baseline
   ↓
change
   ↓
measure
   ↓
target improved AND guard preserved?
   ├── yes ──► keep + evidence
   └── no  ──► revert
```

A ratchet is not:

> “Keep trying until it looks better.”

Autonomous iteration remains bounded, measurable, and reversible.

---

# Blocker convergence

Not every problem is a terminal blocker.

C2GRAPH distinguishes conditions that:

* allow safe continuation;
* require narrowing a claim;
* require replanning;
* require a human decision;
* or genuinely require STOP.

A true gate-red condition fails closed.

A recoverable tooling issue does not have to terminate unrelated work.

See:

Operational blocker-handling contracts are indexed in the [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md).

---

# Handoff, checkpoint, and resume

Long-running agent work eventually crosses:

* context compaction;
* tool budgets;
* rate limits;
* process restarts;
* model/session boundaries;
* human pauses.

C2GRAPH treats continuity as explicit state rather than relying entirely on conversation memory.

A Handoff can preserve information such as:

* active objective;
* Goal;
* current stage;
* completed work;
* remaining work;
* evidence;
* decisions;
* blockers;
* changed paths;
* rollback state;
* next action;
* continuity lineage.

A resumed session is expected to **rehydrate and check divergence** instead of pretending it remembers the previous state perfectly.

Where repository writes are not authorized, continuation can remain outside target bytes rather than allowing a checkpoint to manufacture its own write permission.

---

# Resource epochs

Resource exhaustion is not the same thing as logical task completion.

A run can become:

```text
ACTIVE
   ↓
SUSPENDED / RESUMABLE
   ↓
new resource epoch
   ↓
continue exact remaining work
```

Previously completed discovery or phases are not supposed to restart merely because the host session changed.

See:

Operational budget/resume and context-compaction contracts are indexed in the [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md).

---

# Tool governance

C2GRAPH does not assume documentation accurately reflects real tool usage.

Tool discovery can inspect real call sites such as:

* scripts;
* CI;
* package configuration;
* editor/agent configuration;
* MCP configuration;
* repository configuration;
* other observed invocation surfaces.

This can distinguish:

* tools actually used;
* tools installed but unused;
* conflicting invocations;
* destructive calls;
* ambiguous calls;
* low-confidence findings.

Confirmed observations can support generated runbooks.

Ambiguity remains ambiguity rather than being converted into an invented procedure.

---

# Runtime-scoped model adapter

C2GRAPH v1.3.2 continues to ship **C2ModelAdapter v0.5.5** as the host-side model-adapter boundary for structured model/tool interaction.

The adapter can normalize runtime/provider differences around:

* native tool-call IDs;
* structured arguments;
* JSON-Schema validation;
* streaming assembly;
* runtime-specific parsing;
* required reasoning/provider state;
* capability admission;
* unsupported-combination handling.

Unknown model/runtime combinations are not automatically trusted merely because their names resemble a supported model.

Unsupported or unverified combinations can fail closed.

### Current verified model/provider profiles

C2ModelAdapter v0.5.5 separates the stable API-facing model identifier from the
served model revision when a provider upgrades the backend without requiring a
new public API model string.

* **DeepSeek V4 Pro — V4-Pro-0813.** API calls continue to use
  `deepseek-v4-pro`. The profile covers tool calls, JSON output,
  thinking/non-thinking modes, and provider-scoped reasoning/cache handling.
* **DeepSeek V4 Flash — V4-Flash-0731.** API calls continue to use
  `deepseek-v4-flash`, with the same provider-scoped tool/reasoning/cache
  boundary.
* **OpenAI GPT-5.6 Luna.** `gpt-5.6-luna` and bounded physical aliases such as
  `gpt-5.6-lunaXX` resolve to the Luna tooling/cache family while the physical
  route remains part of cache isolation.
* **OpenAI GPT-5.6 Sol / Terra.** Verified identities remain provider and
  API-surface scoped.
* **xAI Grok 4.5.** `grok-4.5`, `grok-4.5-latest`, and `grok-build-latest`
  use the current verified xAI profile.
* **xAI Grok 4.6.** `grok-4.6*` remains watch/quarantine and fails closed until
  an official 4.6 API/tool/cache contract is verified.

For DeepSeek, **0813 and 0731 are revision labels, not replacement API model
strings**. C2GRAPH therefore does not invent model IDs such as
`deepseek-v4-pro-0813` unless DeepSeek explicitly exposes that identifier in the
API contract.

For heterogeneous router combos, the logical combo name is provenance only.
If a Luna-first combo falls back to an effective DeepSeek route, the adapter
rebinds tooling, reasoning, and cache behavior to DeepSeek and does not replay
Luna-owned Responses item IDs, reasoning state, continuation handles, or cache
state into the DeepSeek attempt.

See:

Adapter implementation and verification surfaces are indexed in the [Script & Verification Catalog](C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md) and source path `adapters/c2modeladapter/`.

---

# Cache-aware adapter

Prompt caching is treated as an optimization—not as authority.

Different providers and serving runtimes expose different cache contracts.

C2GRAPH therefore does not invent one universal:

```text
cache = true
```

abstraction.

The adapter keeps cache behavior provider/runtime scoped while preserving the principle that:

```text
cache hit  ≠ correctness
cache hit  ≠ evidence
cache hit  ≠ freshness
cache hit  ≠ write authority
cache hit  ≠ completion
```

Stable-prefix and cache-scope information can be represented without promoting raw prompt prefixes, private reasoning state, credentials, or raw cache-routing material into durable C2GRAPH authority.

A cache miss must remain a correct execution path.

The intended benefit is **more deterministic cache eligibility and less accidental cache churn**, not a guarantee of provider cache-hit rate.

---

# Tool-call compatibility and validation

A tool call is not one universal wire format.

Model and runtime implementations can differ in:

* argument representation;
* tool identifiers;
* native call IDs;
* parallel calls;
* streaming;
* reasoning/provider state;
* response assembly.

C2GRAPH keeps those differences at the adapter boundary.

A useful separation is:

```text
Can the model express the call?
              ↓
Can the adapter parse it?
              ↓
Does it satisfy the tool schema?
              ↓
Is required runtime state preserved?
              ↓
Is the invocation safe?
              ↓
Is execution authorized?
              ↓
Was it semantically the correct tool?
```

Those are different questions.

A structurally valid tool call does not prove semantic tool selection.

---

# Requirements provenance

Requirements can preserve where they came from.

Examples include:

* extracted;
* inferred;
* ambiguous;
* test-derived;
* document-derived.

This prevents:

> “the agent inferred it”

from silently becoming:

> “the repository requires it.”

Consequential ambiguity remains a human decision boundary.

---

# Tests, documentation, and evidence

C2GRAPH gives executable tests and prose different evidence roles.

A test is not automatically perfect.

Documentation is not automatically authoritative.

When executable behavior and prose conflict, the contradiction should be surfaced rather than silently selecting whichever source is more convenient.

C2GRAPH also distinguishes:

```text
PASS
```

from:

```text
VACUOUS PASS
```

where a check technically succeeds because there was nothing meaningful to inspect.

---

# Provenance

C2GRAPH can record provenance around:

* model;
* provider;
* source;
* run;
* phase;
* decision;
* human acceptance;
* digest lineage.

The provenance ledger can be exported into **W3C PROV-O JSON-LD**.

Digest chaining provides integrity evidence.

It is not presented as cryptographic authorship or identity proof.

---

# Human decision boundaries

Not every engineering decision can or should be automated.

C2GRAPH retains human gates when semantics cannot be established mechanically.

The governing principle is:

> **Automation stops where evidence stops.**

---

# Single-agent and multi-agent operation

C2GRAPH does not assume more agents automatically produce better results.

Before parallelization, the workflow can consider:

* whether success is verifiable;
* whether tasks are independent;
* whether write ownership can be separated;
* whether alternative hypotheses need to remain alive;
* whether persistent state is required;
* whether the latency/cost tradeoff is justified.

Parallel work uses bounded concurrency and an explicit reduction step.

Fan-out does not imply unrestricted concurrent writers.

---

# Mobile-specific governance

C2GRAPH includes dedicated Android and iOS workflow concerns for areas such as:

* build/release;
* device QA;
* release matrix;
* store submission;
* rollback planning.

Mobile release is treated differently because a binary already distributed to users cannot be recovered in the same way as a replaceable server deployment.

---

# Centralized configuration

Operational thresholds belong in:

```text
.agent/config.yml
```

rather than being copied across unrelated prose documents.

This covers areas such as:

* routing;
* retrieval;
* budgets;
* checkpointing;
* ratchet loops;
* concurrency;
* discovery;
* caching/recall;
* governance.

A tunable number should have one owner.

Configuration drift is itself auditable.

---

# Compliance evidence mapping

C2GRAPH includes evidence mappings related to control structures from areas such as:

* SOC 2;
* PCI DSS 4.0;
* EU AI Act.

This is **evidence mapping**.

It is **not certification**.

Using C2GRAPH does not by itself make a system compliant.

---

# Evidence-first reporting

C2GRAPH deliberately limits claims to the evidence that supports them.

Examples:

```text
file count              ≠ token saving
cache telemetry         ≠ reasoning quality
validator PASS          ≠ universal correctness
graph citation          ≠ semantic truth
synthetic simulation    ≠ production behavior
protocol correctness    ≠ better model reasoning
```

The goal is not to eliminate uncertainty from reporting.

The goal is to expose it.

---

### Existing repository + governance only

Use:

Use the shipped `INSTALL-WORKFLOW-ONLY.prompt.md` contract.

and:

The workflow-only entry contract is `INSTALL-WORKFLOW-ONLY.prompt.md`.

### New or empty repository

Select the `BOOTSTRAP` path through the entry-mode decision process.

### Important

Operational authority remains in the matching shipped:

* executable prompts;
* repository rules;
* scopes;
* runbooks;
* configuration;
* verification scripts.

This README explains the system.

It does not override those executable contracts.

---

# Documentation

C2GRAPH v1.3.2 ships a bilingual public-documentation layer that explains the current release without becoming a second operational authority plane.

Operational authority remains in the shipped repository contracts: `AGENTS.md`, `.agent/config.yml`, routed scopes, runbooks, schemas, entry prompts, and executable validators.

The README explains the system. It does not override those contracts.

### Start here

| Need | English | Bahasa Indonesia |
| --- | --- | --- |
| Navigate the public documentation | [Public Docs Index](C2GRAPH-PUBLIC-DOCS-INDEX.md) | [Indeks Dokumentasi Publik](C2GRAPH-PUBLIC-DOCS-INDEX-ID.md) |
| Understand the current release delta | [v1.3.2 Release Overview](C2GRAPH-V1.3.2-RELEASE-OVERVIEW.md) | [Ringkasan Rilis v1.3.2](C2GRAPH-V1.3.2-RELEASE-OVERVIEW-ID.md) |
| Understand Workflow Convergence | [Workflow Convergence](C2GRAPH-WORKFLOW-CONVERGENCE-GUIDE.md) | [Workflow Convergence ID](C2GRAPH-WORKFLOW-CONVERGENCE-GUIDE-ID.md) |
| Understand terminal reconciliation | [Terminal Reconciliation](C2GRAPH-TERMINAL-RECONCILIATION-GUIDE.md) | [Terminal Reconciliation ID](C2GRAPH-TERMINAL-RECONCILIATION-GUIDE-ID.md) |
| Inspect exact distribution paths | [File Map](C2GRAPH-FILE-MAP.md) | [File Map ID](C2GRAPH-FILE-MAP-ID.md) |

### Architecture and source catalogs

| Area | English | Bahasa Indonesia |
| --- | --- | --- |
| Core rules, routes and control-plane inventory | [Control Plane Catalog](C2GRAPH-CONTROL-PLANE-CATALOG.md) | [Katalog Control Plane](C2GRAPH-CONTROL-PLANE-CATALOG-ID.md) |
| Every shipped runbook | [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md) | [Katalog Runbook](C2GRAPH-RUNBOOK-CATALOG-ID.md) |
| Script inventory and registered verification topology | [Script & Verification Catalog](C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md) | [Katalog Script & Verification](C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md) |

### Current measured repository identity

The final v1.3.2 source trees were measured directly rather than inferred from the previous release documentation.

| Surface | English edition | Indonesian edition |
| --- | ---: | ---: |
| Physical files | **248** | **248** |
| Distribution-manifest entries | **239** | **239** |
| Python scripts | **72** | **72** |
| Validator/gate files | **41** | **41** |
| Registered checks | **43** | **43** |
| CORE rules | **40** | **40** |
| Routing rows | **46** | **46** |
| Runbooks including template | **41** | **41** |
| Graph-engineering documents | **28** | **28** |
| Configuration keys | **278** | **278** |
| Node kinds | **20** | **20** |
| Edge kinds | **22** | **22** |

`CORE-40` is Workflow Convergence. It is non-authoritative for repository writes.

### Public-documentation coverage boundary

The public bundle contains **18 Markdown files**: this README pair plus bilingual release overview, Workflow Convergence, Terminal Reconciliation, File Map, Runbook Catalog, Script/Verification Catalog, Control-Plane Catalog, and Public Docs Index.

This public layer is an explanatory/navigation surface. The executable source distribution remains larger and includes internal graph-engineering documents, schemas, runbooks, prompts, scripts, adapter files, and other governance artifacts that are intentionally represented through catalogs rather than duplicated as public prose.

---

# Verification snapshot

C2GRAPH v1.3.2 reports bounded current-release evidence rather than relabeling historical v1.2.0 results as fresh proof.

For both English and Indonesian source editions:

| Surface | Result |
| --- | ---: |
| Registered checks | **43/43 PASS** |
| Failed registered checks | **0** |
| Vacuous pristine-template observations | **4 per edition** |
| Populated aggregate selftest | **0 violations** |
| Terminal directed/contract cases | **39/39 PASS per edition** |
| Built-in finite terminal matrix | **42 states, 0 mismatches per edition** |
| Built-in deterministic successor mutations | **20,000, 0 mismatches per edition** |
| Independent terminal truth matrix | **24 states, 0 mismatches per edition** |
| Independent successor mutation oracle | **100,000, 0 mismatches per edition** |
| Claim-identity chains | **10,000 chains, 0 violations per edition** |
| Real handoff-ledger reuse chains | **250 chains, 0 reuse violations per edition** |
| Flowchart numbered flows | **72** |
| Flowchart structural QA | **PASS** |
| Public-doc structural audit | **PASS** |

The source verifier was replayed from working trees and fresh package extractions. Final packaging acceptance also checks CRC, duplicate/unsafe paths, nested-component parity, and SHA-256 manifests.

Four registered checks can be vacuous on a pristine template because their live corpora are intentionally empty: live HANDOFF, goals log, lessons, and failure recall. Populated selftests exercise those mechanisms separately; the README does not inflate empty-corpus success into behavioral proof.

### Direct upgrade from v1.2.0

Because v1.2.0 is the previous public release, v1.3.2 ships a direct guarded upgrade.

The upgrade is cryptographically bound to the official **237-file v1.2.0 source inventory** preserved in the prior guarded-upgrade lineage.

Current delta:

```text
English
CREATE  11
MODIFY  27
DELETE   0

Indonesian
CREATE  11
MODIFY  26
DELETE   0
```

Source-inventory parity, target parity, already-upgraded detection, tamper rejection, payload CRC, duplicate-path checks, and unsafe-path checks were executed.

The original complete v1.2.0 repository bytes were not available in the artifact environment used for this release build. Therefore the release does **not** claim a fresh physical `v1.2.0 bytes -> apply -> verify -> rollback -> byte-identical v1.2.0` cycle. That boundary is explicit rather than simulated with reconstructed source bytes.

### Evidence boundary

These results are finite deterministic/process/package evidence. They do **not** establish universal bug absence, universal security, arbitrary crash/power-loss safety, distributed-consensus safety, correctness against hostile external concurrent writers, live-provider uptime/correctness, guaranteed cache-hit or cost reduction, improved foundation-model reasoning, or correctness on every future OS/runtime/provider version.

The correct release claim is **bounded verification**, not universal proof.

---

# What C2GRAPH is not

C2GRAPH is **not**:

* a new foundation model;
* a claim that the model becomes intrinsically smarter;
* a universal RAG engine;
* a requirement to deploy a graph database;
* an excuse to give an agent unrestricted write access;
* a guarantee of provider prompt-cache hits;
* proof of lower API cost;
* a semantic wrong-tool-selection solver;
* a replacement for human judgment;
* a compliance certification;
* proof that every possible bug has been eliminated.

C2GRAPH governs how engineering work is represented and executed around a model.

The quality of the underlying model still matters.

---

# Project-agnostic by design

C2GRAPH does not require a particular:

* programming language;
* application framework;
* cloud provider;
* database;
* model vendor;
* repository topology;
* team size;
* agent count.

The governance model is intended to sit around the engineering workflow rather than redefine the product stack.

---

# Research

### Current v1.3.2 release paper

**C2GRAPH: Compositional Repository Governance for Long-Horizon AI Coding Agents**

The current English and Indonesian manuscripts are the **v1.3.2 Release Paper**, publication-dated **3 September 2026 / 3 September 2026**, authored by **Hafizh Albanna**, and archived under:

**Paper DOI:** [10.5281/zenodo.22259065](https://doi.org/10.5281/zenodo.22259065)

The release paper documents the complete Version 1 architecture together with Workflow Convergence, terminal reconciliation, successor admission, current source identity, and bounded release evidence. The paper and software are separate research objects.

### Current v1.3.2 software archive

The C2GRAPH v1.3.2 software/application release is identified by:

**Software DOI:** [10.5281/zenodo.22259056](https://doi.org/10.5281/zenodo.22259056)

The software release is version `1.3.2`, released **3 September 2026**, authored by **Hafizh Albanna**, and distributed under the **MIT License**.

### Previous public release

C2GRAPH v1.2.0, released **22 August 2026**, remains immutable historical provenance. v1.3.1 is the immediate technical predecessor to v1.3.2 but is not presented here as the previous public baseline.

Historical identifiers remain historical records; they are not the canonical v1.3.2 identifiers.

### Citation

If you use C2GRAPH in research, use the repository's `CITATION.cff` for machine-readable software and preferred-paper citation metadata.

Current citation identity:

```text
Software
C2GRAPH v1.3.2
Hafizh Albanna
3 September 2026
DOI: 10.5281/zenodo.22259056

Preferred research citation
C2GRAPH v1.3.2 Release Paper
Hafizh Albanna
3 September 2026
DOI: 10.5281/zenodo.22259065
```

---

# License

C2GRAPH is released under the **MIT License**.

See:

**[LICENSE](LICENSE)**

---

# Author

**Hafizh Albanna**

LinkedIn:

https://www.linkedin.com/in/hafizh-al-banna-63275410

---

## Final principle

C2GRAPH is ultimately built around one idea:

> **An AI agent should not be trusted because it sounds confident.**
>
> **It should be trusted only as far as its objective, evidence, authority, state, provenance, verification, and rollback can be inspected.**

That is **Convert to Graph Engineering**.
