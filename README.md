# C2GRAPH

### Convert to Graph Engineering

**Repository-level governance for long-running AI coding agents.**

C2GRAPH makes agent-driven software work more **bounded, measurable, traceable, resumable, reversible, and auditable** by moving critical execution state out of transient conversation history and into explicit repository contracts.

[![Release](https://img.shields.io/badge/release-v1.0.0-blue)](https://github.com/hf8bz/C2GRAPH/releases/tag/v1.0.0)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Paper DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.21905651-blue)](https://doi.org/10.5281/zenodo.21905651)
[![Software DOI](https://img.shields.io/badge/Software_DOI-10.5281%2Fzenodo.21906534-blue)](https://doi.org/10.5281/zenodo.21906534)

**Version 1 · First public release · 12 August 2026**

[Release Notes](https://github.com/hf8bz/C2GRAPH/releases/tag/v1.0.0) ·
[Research Paper](https://doi.org/10.5281/zenodo.21905651) ·
[Deep Dive](C2GRAPH-DEEP-DIVE.md) ·
[Start Here](C2GRAPH-DOCS-START-HERE.md) ·
[Reference](C2GRAPH-REFERENCE.md)

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

**[Entry Mode Decision Matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md)**

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

**[Actual Write Plan & Rollback Guide](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md)**

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

**[Secret Safety & PHASE 3 Guide](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md)**

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

**[Bounded Read Guide](C2GRAPH-BOUNDED-READ-GUIDE.md)**

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

**[Blocker Convergence Guide](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md)**

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

**[Budget & Resume Guide](C2GRAPH-BUDGET-RESUME-GUIDE.md)**

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

C2GRAPH v1.0.0 includes a host-side model-adapter boundary for structured model/tool interaction.

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

See:

**[Model Adapter Guide](C2GRAPH-MODEL-ADAPTER-GUIDE.md)**

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

# Version 1 verification snapshot

The first public release reports the following measured source shape **per language edition**:

| Surface                     | v1.0.0 |
| --------------------------- | -----: |
| Distributed files           |    198 |
| CORE invariants             |     38 |
| Routes                      |     43 |
| Operational runbooks        |     35 |
| Runbooks including template |     36 |
| Python scripts              |     46 |
| Validators / gates          |     32 |
| Registered checks           |     30 |
| Configuration keys          |    223 |
| Graph Engineering documents |     24 |

Additional release surface:

* **66 flowcharts + index**
* synchronized English and Bahasa Indonesia editions
* operator and architecture documentation
* reproducibility evidence
* formal papers
* standalone model-adapter reference

Completed release verification includes:

| Verification                        |                                     Result |
| ----------------------------------- | -----------------------------------------: |
| Registered checks                   | **30/30**, 0 observed failures per edition |
| Bounded Read selftest               |                                  **56/56** |
| Model-adapter admission selftest    |                                  **23/23** |
| Standalone adapter pytest           |                                  **33/33** |
| Randomized structural adapter cases |                          **20,000/20,000** |
| Randomized cache-plan cases         |                          **10,000/10,000** |
| Flowchart compilation               |                      **66/66**, 0 warnings |

Four clean-template observations remain explicitly classified as **VACUOUS** because a clean template does not contain live Goal, Handoff, Lesson, or Failure records.

These results establish behavior only inside the executed deterministic/process-level test surfaces.

They do **not** prove:

* universal bug absence;
* correctness across every repository topology;
* semantic model quality;
* guaranteed provider cache-hit rates;
* universal token or cost reduction;
* production latency improvement;
* perfect wrong-tool selection;
* certification or regulatory compliance.

For the detailed release evidence and boundaries, see:

**[C2GRAPH v1.0.0 Release Notes](https://github.com/hf8bz/C2GRAPH/releases/tag/v1.0.0)**

---

# Quick start

C2GRAPH intentionally has multiple entry paths.

Do not begin by assuming every repository needs the same workflow.

### Existing repository + implementation work

Start with:

**[Entry Mode Decision Matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md)**

Then use the adoption path corresponding to `ADOPT_WITH_GOAL`.

### Existing repository + governance only

Use:

**[Workflow-Only Guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md)**

and:

**[Workflow-Only Agent Instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md)**

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

| I need to...                              | Read                                                                              |
| ----------------------------------------- | --------------------------------------------------------------------------------- |
| Start from the correct entry mode         | [Entry Mode Decision Matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md)               |
| Install only the governance/control plane | [Workflow-Only Guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md)                             |
| Give an agent a workflow-only instruction | [Workflow-Only Agent Instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md)     |
| Understand the architecture               | [Deep Dive](C2GRAPH-DEEP-DIVE.md)                                                 |
| Find exact rules, gates, or thresholds    | [Reference](C2GRAPH-REFERENCE.md)                                                 |
| Navigate all documentation                | [Docs Start Here](C2GRAPH-DOCS-START-HERE.md)                                     |
| Read large/minified/log evidence safely   | [Bounded Read Guide](C2GRAPH-BOUNDED-READ-GUIDE.md)                               |
| Understand model/cache/tool compatibility | [Model Adapter Guide](C2GRAPH-MODEL-ADAPTER-GUIDE.md)                             |
| Review exact writes and rollback          | [Actual Write Plan & Rollback Guide](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md) |
| Resolve blockers correctly                | [Blocker Convergence Guide](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md)                 |
| Handle session/resource exhaustion        | [Budget & Resume Guide](C2GRAPH-BUDGET-RESUME-GUIDE.md)                           |
| Handle secret exposure                    | [Secret Safety Guide](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md)                      |
| Resolve host Python launcher notation     | [CLI Launcher Guide](C2GRAPH-CLI-LAUNCHER-GUIDE.md)                               |
| Diagnose a stopped run                    | [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md)                                     |
| Find a shipped file                       | [File Map](C2GRAPH-FILE-MAP.md)                                                   |

---

<details>
<summary><strong>Full C2GRAPH v1.0.0 capability map</strong></summary>

Version 1 includes the following major capability families:

1. Three-layer context architecture
2. Three explicit entry modes
3. Goal Contract
4. Exact Write Plan and rollback authority
5. Mutation-free GATE 0
6. Secret-aware discovery
7. Bounded Evidence Read v2
8. Bounded repository scanners
9. Corpus partitions and retrieval discipline
10. Per-application index boundaries
11. Typed Graph Plane
12. Progressive query architecture
13. Durable Lessons
14. Failure Memory and pre-action recall
15. Ratchet loops
16. Workflow discipline and rulebook self-checking
17. Blocker convergence
18. Context-compaction continuity
19. Resource-epoch resume
20. Checkpoints as graph lineage
21. Tool-usage capture from real call sites
22. Test-to-workflow mapping
23. Requirements provenance
24. Human decision boundaries
25. Single-agent / multi-agent architecture selection
26. Bounded agent dispatch
27. Runtime-scoped model adapter
28. Cache-aware adapter
29. Mobile-specific release governance
30. Provenance and W3C PROV-O export
31. Compliance evidence mapping
32. Centralized configuration
33. Evidence-first reporting
34. Bilingual release surface

See the full explanations in the:

**[v1.0.0 Release Notes](https://github.com/hf8bz/C2GRAPH/releases/tag/v1.0.0)**

</details>

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

### Research paper

**C2GRAPH: A Repository-Level Governance Package for Disciplined, Measurable AI Coding Agents**

DOI:

**https://doi.org/10.5281/zenodo.21905651**

### Software archive

C2GRAPH v1.0.0:

**https://doi.org/10.5281/zenodo.21906534**

### Citation

If you use C2GRAPH in research, please use the repository's:

**[CITATION.cff](CITATION.cff)**

---

# Release

**C2GRAPH v1.0.0 — Version 1 / First Public Release**

Full release notes:

https://github.com/hf8bz/C2GRAPH/releases/tag/v1.0.0

Repository:

https://github.com/hf8bz/C2GRAPH

---

# License

C2GRAPH is released under the **MIT License**.

See:

**[LICENSE](LICENSE)**

---

# Author

**Hafizh Al-Banna**

LinkedIn:

https://www.linkedin.com/in/hafizh-al-banna-63275410

---

## Final principle

C2GRAPH is ultimately built around one idea:

> **An AI agent should not be trusted because it sounds confident.**
>
> **It should be trusted only as far as its objective, evidence, authority, state, provenance, verification, and rollback can be inspected.**

That is **Convert to Graph Engineering**.
