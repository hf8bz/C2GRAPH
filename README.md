# C2GRAPH v1.0.0 — Version 1, Adapter-Integrated Full Release

This is still **C2GRAPH v1.0.0 / Version 1**. The host model-adapter integration does not create a second product version. The current source tree contains **198 files per edition**, **46 Python scripts**, **32 validators/gates**, **30 registered checks**, **43 routes**, and **CORE-01..CORE-38**.

The new host boundary uses C2ModelAdapter **v0.4.1** internally. Cache reuse is an optimization, never goal/evidence/write/completion authority. See `C2GRAPH-MODEL-ADAPTER-GUIDE.md`. The final bounded audit reports zero known unresolved defect/ambiguity inside the explicitly executed deterministic/process-level scope; it does not claim universal bug absence or live-provider semantic correctness.

# C2GRAPH v1.0.0 - First Public Release

> The initial public baseline is v1.0.0. This document preserves its historical audit sections while
> current installation and measured-state instructions describe the first-public v1.0.0 release baseline, dated **12 August 2026**.
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

[Docs start](C2GRAPH-DOCS-START-HERE.md)

**Make your AI coding agent measurable, resumable, and honest - in any repository, in any language,
on any CLI or IDE.**

Unpack the 174-file first-public v1.0.0 distribution beside your repo, prove the write boundary, then choose one of three entry prompts. Existing repositories can now
install the C2GRAPH workflow without creating or reactivating a goal; product implementation remains
strictly goal-bound. From then on your agent
cites file and line for every claim, survives context compaction without redoing work, and, on goal-emitting paths, hands you a `/goal` block you can paste to keep implementation running
end to end.

**C2GRAPH is short for Convert to Graph Engineering.**

Available in **English** and **Bahasa Indonesia**. Repository: **`hf8bz/C2GRAPH`**.

> **This document is the pre-public release manual for the intended first public v1.0.0 release. Pre-release engineering snapshots are evidence history, not prior releases.**
> Workflow-only installation, goal-bound adoption, and new-repository bootstrap all ship in this
> initial baseline. Every audit round described later was a pre-release development/audit round, not
> an installable public version. Current install steps, entry modes, measured counts, thresholds, and
> gates are described here. Rule identifiers are permanent and are not reused or renumbered. Sixteen
> companion documents provide navigation, workflow-only operations, rollback/write-plan safety, secret
> safety, reference, troubleshooting, deep-dive material, and the complete file map.

---


## v1.0.0 - First public release (12 August 2026)

The first public release includes a stateless bounded text-read substrate, generation-bound source-byte + line/character continuation, three-window strategy review, typed read
outcomes, conservative Unicode path recovery, special-device/binary routing, and bounded census/plan scanner
integration. The measured EN/ID tree is 174 files, 43 Python scripts, 30 validators, 28 registered checks, 211 config
keys, and 132 thresholds. Those inventory counts are release measurements; no production token or answer-quality gain
is inferred from them.

## Dedicated workflow-only operator documentation

The first-public-release documentation bundle separates workflow-only and safety material into purpose-built documents so the operator contract is not buried inside the release post or troubleshooting prose:

- [`C2GRAPH-WORKFLOW-ONLY-GUIDE.md`](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) -- canonical end-to-end operator guide: intent, pre-write evidence, six goal states, allowed/forbidden writes, delegated SCAN semantics, gate modes, PHASE 8, final report, STOP rules, follow-up goal creation, and scenario walkthroughs.
- [`C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md`](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) -- pre-authority zero-mutation barrier, static S1-S4 tool discovery, preliminary rollback-basis scan, incident recovery, and regression matrix.
- [`C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md`](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md) -- exact `ACTUAL_WRITE_SET`, path-scoped rollback authority, semantic-scope separation, decision persistence, `distribution_restore`, and legitimate PHASE 3.W blockers.
- [`C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md`](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md) -- deterministic false/circular-vs-legitimate blocker routing, no-op elimination, explicit MKDIR freshness, named rollback-unit continuation, and end-to-end blocker convergence.
- [`C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md`](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) -- complete copy-paste instruction for a coding agent plus post-run acceptance checks.
- [`C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md`](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) -- deterministic routing tables for `ADOPT_WITH_GOAL`, `WORKFLOW_ONLY`, and `BOOTSTRAP`, including mixed requests and every workflow-only goal state.

These documents explain the executable repository-root prompt; they do not supersede it. `INSTALL-WORKFLOW-ONLY.prompt.md` remains the executable source of truth.

### CORE-33 secret-safe discovery and PHASE 3 routing

Secret-prone inputs such as `.env*` and `wrangler*.toml` are routed through
`scripts/scan/redact.py`, which emits structural line plus identifier-class/fingerprint evidence without raw values or raw identifiers,
subprocesses, network access, or target writes. If a value nevertheless reaches context during
PHASE 0-2, C2GRAPH contains and quarantines the unsafe slice, stages sanitized
`PENDING_SECURITY_INCIDENT`, and continues only independent safe read-only discovery. **PHASE 3 is the
mandatory human stop for CORE-33**, so a discovery line such as PHASE 1.13 does not end the run merely
because the exposure occurred there. Exposure after PHASE 3 returns to that barrier. Other independent
STOP rules are unchanged.

See [C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md).

## What is in v1.0.0, and why the checks still matter

The initial `v1.0.0` baseline ships three entry paths from day one. The third is for an existing
repository whose previous goal is complete, whose goal ledger does not exist yet, or which otherwise
needs the C2GRAPH workflow installed, repaired, upgraded from a development/local copy, or verified
before the next objective is known. `INSTALL-WORKFLOW-ONLY.prompt.md` preserves the goal plane,
changes zero product behaviour, and emits no goal. The pre-release audit record remains below;
current counts come from the exact v1.0.0 release tree.

An external ultra-detail audit ran against a late pre-release in devil's advocate mode: six critical,
fifteen high and twenty-two medium findings, then a third adversarial round by independent checkers
that found defects in the fixes themselves. The single most important finding was not a wrong number.
It was that **nothing in the package could check any of its own claims**, so drift was undetectable by
design, and every number printed here was an assertion.

That remains closed. The candidate ships **30 validators/gates** plus shared helpers, four target-read-only
scanners, one provenance exporter, interview/state/install helpers, and the self-test harness - **43
Python files** in total. `<C2PY> scripts/verify/all.py` runs **28 registered checks** and fails closed:

| Check | What would make it fail |
| --- | --- |
| `verify/counts` | nothing; it is the measurement every other check reads |
| `verify/claims` | a published number that no longer matches the tree, or a token with no dictionary row |
| `verify/paths` | a path literal that does not resolve and is not declared as a runtime artifact |
| `verify/links` | a relative Markdown link that leads nowhere |
| `verify/excludes` | exclude rules that contradict the carve-outs needing read access |
| `verify/width` | the router past `router_max_lines`, or prose past `prose_max_line_length` |
| `verify/hygiene` | a non-ASCII character, a tab, trailing whitespace, CRLF, a BOM, a missing final newline, or a `.pyc` left behind by the suite itself |
| `verify/cli` | the documented command line and the implemented one disagreeing, in either direction |
| `verify/provenance` | the decision ledger and its schema disagreeing, a broken digest chain, or an accepted record with no named acceptor |
| `verify/placeholders` | an unresolved placeholder, in either language's marker form |
| `verify/index_scope` | a code index at the repository root of a monorepo, or an application with no index (CORE-34) |
| `jsonl/validate` | a malformed line or a missing required key |
| `graph/validate` | an undeclared node or edge kind, a dangling endpoint, a dishonest emitted count |
| `handoff/validate` | the schema, the live checkpoint and the canary nonce disagreeing |
| `goal/gate` | a goal condition failing, reported with counts instead of a verdict |
| `lessons/decay` | a lesson older than `lesson_reconfirm_interval_days` with no reconfirmation |
| `verify/compliance` | the control map, evidence paths, or framework identifiers drift |
| `verify/preserve` | the frozen CORE baseline, registry, and router stop reconciling |
| `verify/recall` | failure-memory retry or promotion obligations are violated |
| `verify/discipline` | entry-mode, blocker, workflow, or goal-discipline contracts drift |
| `verify/enforcement` | mechanical CORE coverage falls below the ratchet floor |
| `verify/routes` | Markdown and YAML routing tables diverge or point at missing targets |
| `verify/prefix` | the stable router skeleton differs from the deliberate lock |
| `verify/golden` | retrieval golden-set shape or coverage falls below its floor |
| `verify/ambiguity` | advisory language appears in a normative rule surface |
| `selftest` | validators pass only vacuously and fail on populated fixtures |

So the honest reading of this page is: **do not believe the numbers here, run the checks.** If a
number in this document disagrees with `<C2PY> scripts/verify/counts.py --json`, the script is right
and the document is stale.

The measured shape of the `v1.0.0` first-public-release tree, printed by that script:

| Measurement | Value |
| --- | --- |
| files per edition | **174** |
| invariants | `CORE-01..CORE-38`, contiguous |
| routing rows | **42** |
| router | **195 lines English / 194 lines Indonesian**; limit `router_max_lines: 195` |
| scopes | 11 files, 10 examples plus a template |
| runbooks | 35 files, 34 procedures plus a template |
| prompts | 12 study prompts, plus **3 entry prompts** |
| config | **211 keys**, of which **132** are thresholds |
| graph vocabulary | **20 node kinds**, **22 edge kinds** |
| gates | `G0..G36` plus `G1b` and `G28b` (**39**), `N0..N32` plus `N1b` and `N24b` (**35**) |
| placeholder tokens | 54, each with a dictionary row |
| shipped code | 43 Python scripts; 30 validators/gates; 28 registered checks; 4 scanners; 1 exporter |
| workflow-only goal-state self-test | 5 cases: absent, empty, terminal-only, active, conflicting active goals |

Five things in that list exist because of findings serious enough to name:

1. **The archives contain a wrapper folder.** Unzipping straight over a repository root was the
   critical finding: it silently overwrote an existing `AGENTS.md`. Keep the unpacked distribution
   outside `TARGET_ROOT`. PHASE 0 preflight is a read-only rollback-basis scan that reports
   `pending_write_plan`; after PHASE 3 decisions, plan-specific preflight classifies only the exact
   `ACTUAL_WRITE_SET`. Unrelated identical or differing collisions do not create authority or blockers.
2. **Router paths resolve.** Every `runbooks/` and `scopes/` reference carries the `.agent/` prefix it
   always needed, and all path literals are checked by `verify/paths`.
3. **Concurrency defaults to 1 worker**, because the state design has a single writer. Raising it is a
   decision you make with the lease and the handoff in front of you, not a default you inherit.
4. **State is durable on purpose**: an event-sourced handoff, a TTL lease in `LOCK.json` that makes
   split brain detectable, a canary nonce that makes a stale resume detectable, and a migration script
   for the schema version.
5. **CORE-34, index placement in a monorepo.** If a repository holds several applications and a code
   index is built once at the repository root, every query answers with matches from every
   application. That noise is not cosmetic: it is what turns a located claim into a plausible one. A
   monorepo carries one index per application, bound to the partition it indexes. A single root index
   is legal only for a single-application repository, and only when that is written down as
   `placement: single_app` with `root_index_allowed: true`. The rule ships as five parts rather than a
   paragraph: the invariant in `AGENTS.md`, the procedure in
   `.agent/runbooks/monorepo-index-placement.md`, the `index:` declaration in `.agent/config.yml`, the
   validator `scripts/verify/index_scope.py`, and a routing row so an agent that never read this page
   still finds the rule.

Governance files that a template of this kind is usually only claimed to have are present and
readable: `LICENSE` (MIT), `SECURITY.md`, `CONTRIBUTING.md`, `CODEOWNERS`,
`docs/HOST-COMPATIBILITY.md`, and a runbook for treating repository content as untrusted data
(CORE-32).

Two items stay open, and are documented instead of claimed:

- **Host coverage.** `docs/HOST-COMPATIBILITY.md` records the hosts actually exercised, with dates.
  Anything not listed there is untested, not "supported".
- **Authenticity.** The checksums below prove the file is intact. They prove nothing about who
  produced it. There is no signature, and a checksum published beside its own download is not one.

## Downloads

| Language | File | Size | Files |
| --- | --- | --- | --- |
| English | `C2GRAPH-en-v1.0.0.zip` | see release checksum/manifest generated from the final freeze | 174 |
| Bahasa Indonesia | `C2GRAPH-id-v1.0.0.zip` | see release checksum/manifest generated from the final freeze | 174 |

Every one of the 174 files is listed with its purpose on a separate page:
**[C2GRAPH - file map](C2GRAPH-FILE-MAP.md)**. It is kept out of this document on purpose, because
you need it once when you are deciding what to delete, and never again.

Each archive contains a single top-level folder, `C2GRAPH-en/` or `C2GRAPH-id/`. That is
deliberate: a pre-release unzipped straight over your repository root and could overwrite an existing
`AGENTS.md` without asking, which was the most serious finding of the audit. Unzip anywhere, then
let `scripts/install/preflight.py` tell you what would collide before you copy anything.
Both trees are structurally aligned: the same 174 distribution paths and rule IDs are present. Localized prose
differs.

Check the archive before you unzip it:

```bash
sha256sum -c <<'EOF'
f09e63a143b9556d9b3ebffc4e365d5747ab9ce9d8cc535b15279122a8c302ee  C2GRAPH-en-v1.0.0.zip
0a038d58e29d85c5183e0c735748a9a493817c84274f1e03c9ad315c542b7993  C2GRAPH-id-v1.0.0.zip
EOF
```

---

## Contents

This page is the release post and the getting-started guide. Sixteen companion documents hold
material you look up by task rather than read start to finish. Together with this page they form the
fifteen-file documentation bundle described in `C2GRAPH-DOCS-START-HERE.md`.

**Start here**
[What is in this candidate](#what-is-in-this-candidate) - [The name, unpacked](#the-name-unpacked) -
[Is this for me?](#is-this-for-me) - [Tools you need](#tools-you-need) -
[Install in 60 seconds](#install-in-60-seconds) - [Your first run](#your-first-run) -
[What you will be asked](#what-you-will-be-asked) - [What gets created](#what-gets-created)

**Daily use**
[Command cheat sheet](#command-cheat-sheet) - [When your CLI compacts](#when-your-cli-compacts) -
[The goal block](#the-goal-block) - [Reading the status lines](#reading-the-status-lines) -
[After the goal is done](#after-the-goal-is-done)

**Why it works**
[The problem it solves](#the-problem-it-solves) -
[Three layers of context](#three-layers-of-context) -
[What AGENTS.md does](#what-agentsmd-actually-does) -
[Why it costs less to run](#why-it-costs-less-to-run) -
[Where this came from](#where-this-came-from) -
[The audit record](#the-audit-record-folded-into-one-release) -
[Where this sits](#where-this-sits) -
[The case against adopting it](#the-case-against-adopting-it)

**Closing**
[Known limits](#known-limits) - [Design notes](#design-notes) - [Feedback](#feedback) -
[Licence](#licence)

### Companion pages

| Page | What is on it | When you open it |
| --- | --- | --- |
| [Docs start](C2GRAPH-DOCS-START-HERE.md) | task-oriented navigation across the documentation bundle | first, when you are not sure which page to open |
| [Workflow-only guide](C2GRAPH-WORKFLOW-ONLY-GUIDE.md) | end-to-end workflow-only contract, state handling, scope, PHASE 8, scenarios | when installing/repairing/upgrading/verifying C2GRAPH without goal creation |
| [Safe GATE 0](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md) | zero-mutation discovery, rollback authority, preflight, pending state, and recovery | before pre-authority discovery or recovery from an unsafe probe |
| [Actual write plan / rollback](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md) | exact `ACTUAL_WRITE_SET`, path-scoped rollback authority, false-blocker elimination, and restore candidates | when PHASE 3 reports collisions, pending decision persistence, or asks for a snapshot |
| [Blocker convergence](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md) | stop/continue matrix across persistence, tooling, CORE-33, no-op, MKDIR, snapshot/VCS, and scope | when a run reports a blocker and you need to decide whether it is real |
| [Secret safety / PHASE 3](C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md) | CORE-33 redaction, containment, mandatory PHASE 3 decision, and reporting | whenever a repo may contain credentials or a value was accidentally exposed |
| [Agent instruction](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md) | complete copy-paste instruction plus acceptance checks | when you are about to give the task to an agent |
| [Entry-mode matrix](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md) | deterministic routing for all three entry modes and workflow-only goal states | when choosing an entry or resolving an edge case |
| [Deep dive](C2GRAPH-DEEP-DIVE.md) | the accuracy layer, compaction handoff, goal contract, workflow discipline, mobile, tool practice, tests, plans | when you want to know how a subsystem actually behaves |
| [Reference](C2GRAPH-REFERENCE.md) | the 38 invariants, every configuration key, placeholders, graph vocabulary, gates, maturity levels, terminology | when you need an exact name, number or threshold |
| [Troubleshooting](C2GRAPH-TROUBLESHOOTING.md) | symptoms, causes, fixes, and the questions people actually ask | when something is not behaving |
| [File map](C2GRAPH-FILE-MAP.md) - [Bounded read](C2GRAPH-BOUNDED-READ-GUIDE.md) | all 174 current files, each with its purpose | once, when deciding what to delete |

---

## What is in this candidate

This release assembles the first public C2GRAPH release in one place. There are no earlier public C2GRAPH releases to
reconstruct. References to earlier rounds or revisions describe pre-release development work that
was folded into the `v1.0.0` baseline before the 12 August 2026 first public release.

**What you get.** A router your agent reads on every turn, a set of runbooks it loads only when the
matching work actually starts, scopes for narrow tasks, three entry prompts - goal-emitting adoption,
workflow-only installation, and new-repo bootstrap - eleven reusable study prompts, nineteen numbered
studies behind the design decisions, a graph vocabulary of twenty node kinds and twenty-two edge kinds,
and the schemas that keep the agent's own memory honest.

**What the corrections have been about.** Every round of work on this template has fixed the same
kind of defect rather than adding features:

- **Numbers that drifted apart from the config.** A rule that says 150 while the config says 170
  teaches the agent the wrong limit, and nobody notices because both files look authoritative. Every
  number stated in prose is now checked against the key it claims to describe, and the key is named
  next to the number so a future change has one obvious place to land.
- **The two language trees disagreeing in substance.** Translation drift is easy to miss because
  both files read fluently on their own. The trees are now compared structurally - same paths, same
  rule IDs, same thresholds, same table shapes - so a paragraph cannot quietly lose a threshold in
  one language while keeping it in the other.
- **Schemas that forbade what the runbooks write.** A closed enum in a schema, and a runbook writing
  values outside it, produces a validator that rejects correct output. Schema and usage are now
  reconciled in both directions.
- **Terminology splitting into synonyms.** One concept described by two words costs the reader an
  inference on every encounter. A shipped script now enforces the canonical vocabulary, with the
  deliberate exceptions written down instead of remembered.
- **Claims in this document that were never executed.** The verification commands printed below are
  run against the exact tree inside the archive before release packaging, and the checksums are taken from
  the exact files intended for the first public release. A number in this post that you cannot reproduce is a
  defect, and it is treated as one.

- **Sentences long enough to hide an obligation.** A rule that packs six conditions into one
  clause chain is technically complete and practically unreadable, and the reader who skims it
  misses a condition. Long sentences are now measured rather than judged by feel, and the two
  language trees are compared against each other: when one of them states a rule in two sentences
  and the other needs fifty words, the longer one is the defect.

**Names that only line up in one language.** The device QA matrix has five slots, keyed in the
configuration file as `floor`, `volume`, `current`, `large`, and `adverse`. The English guide named
them the same way, so the mapping was invisible and therefore never questioned. The Indonesian guide
translated them, which reads better but left no way to tell which table row belonged to which
configuration key. Identifiers are now printed beside the translated names, so a reader in either
language can fill the file without guessing.

**A name that was only obvious to one kind of reader.** The device-slot table labelled its rows in
words, and one language happened to use words that matched the configuration keys exactly. That
reader could fill in the configuration; the other had to guess which row went with which key. Both
tables now print the identifier beside the label, so neither reader has to infer anything.

**A checklist that measured something before it was finished.** One guide asked the reader to check
the router file against its line limit before the step that removes an unused feature, and that step
deletes routing rows. A reader could fail a check against rows they were about to delete, then go
trimming a file that was never too long. Ordered work now runs in the same order in both guides.

**A count that described only half of a list.** These pages described the script contracts with a
number that counted the second table and quietly ignored the first. The contracts themselves were
always complete; the summary of them was not.

**What has not changed.** The archives still carry no dependency, no install step and no runtime,
and every rule identifier is still permanent. What did change: they now unpack into a wrapper
folder and contain 174 files, and still carry no dependency, no install step, and no runtime. It
is a set of Markdown files and a handful of shell scripts. You can read all of it.

---

## The name, unpacked

**C2GRAPH is short for Convert to Graph Engineering.** Three words, and each one is load bearing.

**Convert.** You are not starting a new project. You already have a repository with history,
conventions, a half-finished migration and a pile of knowledge that lives in people's heads.
C2GRAPH converts what is already there into something an agent can navigate. It does not ask you to
rewrite anything, and the adoption prompt stays read-only until you have approved what it found.

**Graph.** What the agent learns is stored as a small graph: twenty kinds of node, twenty-two kinds
of edge. A run produced an artifact. An artifact is evaluated by a metric. A lesson is contradicted by
a later test. A goal constrains a task. That shape is what lets the agent answer *why is this here*
months later, instead of re-deriving the answer at full price in every session. Prose forgets. A
graph does not.

**Engineering.** Everything here is testable. Every runbook ends with a condition you can observe.
Every claim has to name the file and the line it came from. Every gate is binary, and the package
ships the commands you use to check your own copy. There is no "looks fine" state anywhere in it.

---

## Is this for me?

**Yes, if you recognise any of these:**

- Your agent says *"I've updated the authentication logic"* and you have no idea which file it means.
- Your CLI compacts the conversation and the agent cheerfully re-reads the same 30 files.
- You explain the same project rules at the start of every session.
- Your root instruction file has grown substantially and your host includes that root policy in each request, increasing always-loaded context.
- The agent reports success, and the tests were never run.
- You ship mobile and the agent treats a store release like a web deploy.

**Probably not, if:**

- You use an agent for one-off scripts and throwaway work. The setup cost will not pay back.
- You want something that works with zero configuration. This asks you ~25 questions once, on purpose.

**Works with:** Claude Code, Cursor, Windsurf, Cline, Aider, Copilot Workspace, Gemini CLI, Codex CLI,
Zed, Continue - and any agent that reads a rules file. Nothing here is tied to a single vendor.

**Works for:** web apps, backend services, libraries and SDKs, CLIs, infrastructure repos, data and ML
projects, monorepos, internal tooling, and **Android and iOS apps**.

---

## Tools you need

**For the shipped C2GRAPH control plane, Python 3 is the execution prerequisite.** `git` is strongly
recommended but is not an unconditional installer prerequisite: a target without `.git` may still
proceed when install preflight proves the exact destination set is `new_files_only`, or when another
named rollback authority is accepted. Archive extraction (`unzip`/`tar` or an equivalent host action)
prepares `C2GRAPH_SOURCE_ROOT` **outside** the target and is not a GATE 0 target probe.

Tool categories describe what evidence or later product action becomes unavailable; they do **not**
grant permission to execute a candidate tool during GATE 0.

| Category | Meaning | GATE 0 consequence when unverified/unavailable |
| --- | --- | --- |
| **REQUIRED** | C2GRAPH itself cannot perform the required control-plane step without it | stop only the step that truly requires it; do not bootstrap it implicitly |
| **RECOMMENDED** | a weaker/read-only alternative or narrower claim exists | use the alternative and record a claim limit |
| **OPTIONAL** | one capability is unavailable | record the capability/claim limit; do not manufacture availability |

### The important tools

| Tool | Category | What it provides | Safe GATE 0 interpretation |
| --- | --- | --- | --- |
| `<C2PY>` | REQUIRED | runs shipped scanners, preflight, validators, and self-tests | the interpreter already running `scan/tooling.py` is execution-proven; no second version probe is needed |
| `git` | RECOMMENDED | provenance, history, and a convenient rollback unit | pre-authority census does not launch Git; only safe static `.git/HEAD`/ref metadata is read. Existing Git becomes a rollback candidate only after human HEAD/worktree attestation, then authorized `V` before first write |
| `rg`, `grep`, `find`, `fd` | RECOMMENDED | bounded text/file discovery | prefer host/distribution-owned read primitives; do not execute an untrusted project shim |
| `jq`, `yq` | RECOMMENDED/OPTIONAL | structured-data inspection | a PATH hit is only `on_path_unverified` until later approved execution |
| `tokei`, `scc`, `cloc` | RECOMMENDED | measured partition denominators | defer execution until authority/need if not already trusted |
| `duckdb`, `sqlite3` | OPTIONAL | multi-hop graph queries | lack of execution proof narrows claims; it does not block workflow installation |
| `gitleaks`, `trufflehog` | OPTIONAL | secret-scanner evidence | run only in an authorized lane with declared side effects |
| target package managers/runners | PRODUCT-SPECIFIC | tests/build/deploy for the target | declarations are S3/S4 evidence only; never fetch/execute them merely to discover availability |

Shipping a mobile app still requires its platform toolchain for the **mobile product lane**, not for a
workflow-only installation. Android/iOS tools are verified only when that later authorized action
needs them.

### Safe PHASE 0 inventory

Run the shipped scanner from the trusted source tree; it performs PATH resolution without launching
candidate tools:

```bash
cd "$C2GRAPH_SOURCE_ROOT"
<C2PY> scripts/scan/tooling.py --target "$TARGET_ROOT"
```

The important statuses are `present` for the already-running Python interpreter,
`on_path_unverified` for a resolved PATH entry, and `not_on_path` when S1 found no path. S2-S4 may
later add `present_local_unverified`, `runner_declared_unverified`, or `declared_not_installed`.
`broken`, `available_via_runner`, and execution versions require an explicitly approved
post-authority verification step `V`; GATE 0 does not create them by running `--version`, `--help`,
or an executable `git`. Git repo evidence before authority is static `.git/HEAD`/safe in-tree ref
metadata only; cleanliness and commit count remain unknown until authorized `V`.

Do **not** use `npx`, `npm exec`, `pnpm exec/dlx`, `yarn dlx`, `bunx`, `uvx`, `pipx`, `corepack`,
`go run`, a project-local binary, arbitrary `<tool> --version`, arbitrary `<tool> --help`, or a
network request merely to discover availability. Those commands can fetch packages, create caches,
run hooks, or touch the workspace.

The inventory is first held as session evidence. `.agent/tooling.yml` is persisted only when its exact
post-3.W destination/action is semantically required, appears in `ACTUAL_WRITE_SET`, has path-scoped
authority, and is fresh. Failure/discipline/cache/handoff records likewise remain `PENDING_*` unless
their own exact persistence action independently passes those four conditions.

> **The agent never installs anything on its own (CORE-15).** An install offer is data, not
> permission. It must name the cost of skipping, alternative, pinned command/source, expected network
> and filesystem effects, verification step, and rollback. Actual execution requires the applicable
> human approval and rollback authority.

---

## Install safely

### Step 1 - unpack beside the target repository

Do **not** unzip the archive over the repository root. Keep C2GRAPH as a trusted source tree outside
the target until rollback/write authority is proven.

```bash
cd ~/Downloads
unzip C2GRAPH-en-v1.0.0.zip
# or: unzip C2GRAPH-id-v1.0.0.zip

C2GRAPH_SOURCE_ROOT="$HOME/Downloads/C2GRAPH-en"
TARGET_ROOT="/path/to/your-repo"
```

### Step 2 - run target-byte-read-only preflight from the source tree

```bash
<C2PY>   "$C2GRAPH_SOURCE_ROOT/scripts/install/preflight.py"   --template "$C2GRAPH_SOURCE_ROOT"   --target "$TARGET_ROOT"
```

Do not follow preliminary preflight with a blind `cp -a`, `cp -f`, archive extraction over the target,
or `rsync --delete`. This PHASE 0 call is read-only basis inventory and must report
`rollback_candidate pending_write_plan`; collision counts are not final authority. PHASE 0-2 remain
target-byte read-only while human answers and audit evidence stay in session/host state.

After mutation-affecting PHASE 3 decisions are known, construct exact `ACTUAL_WRITE_SET` and run
plan-specific preflight with repeated `--plan-op ACTION:PATH`, or `--plan-empty` for a zero-write
verification. Planned CREATE paths may use `new_files_only`; preflight derives missing parent directories
as exact `MKDIR` fingerprint/rollback rows with `EXPECT_DIR_ABSENT`; each parent is verified and created top-down as an explicit action before CREATE. Planned MODIFY/DELETE paths that are byte-identical to the same path in an independent C2GRAPH source may use `distribution_restore`.
Only an actual planned customized existing-file change without an independent before-image needs a
snapshot/VCS rollback unit. Unrelated collisions do not. Symlink/path-shape and hardlinked existing
write targets are fail-closed. Rollback safety never grants semantic scope.

Record `write_plan_sha256`, `target_state_sha256`, the rollback manifest digest, and each emitted
`EXPECT_ABSENT` / `EXPECT_SHA256` row. Rerun the same plan-specific preflight immediately before the
first write; fingerprint drift invalidates the transaction-start authority. Then run
`--verify-op ACTION:PATH --expect-state ...` immediately before every individual mutation and require
`freshness_verified yes`. The per-write guard narrows multi-file TOCTOU exposure but is not an OS
transaction lock.

### Step 3 - use an entry prompt; let the workflow instantiate the authorized set

For an existing repo, the selected entry prompt reads the repository before installation. Until
rollback authority exists, GATE 0 uses static S1-S4 evidence only and target bytes remain unchanged.
No early failure ledger, tooling file, draft, cache, lock, handoff, generated document, or runner
artifact may be written merely to record progress.

If the target has no `.git`, that alone is not a failure. Final authority is determined from the exact
actual write plan: zero writes need no rollback unit; CREATE-only plans may use `new_files_only`;
independent distribution-backed existing-file restores may use `distribution_restore`; only uncovered
planned existing-file mutation requires a broader external rollback unit. The automatic
`snapshot_or_vcs_required` candidate is not expected to disappear after Q-ROLLBACK: the named unit is
bound to exact covered paths and `write_plan_sha256` as `proven:<named-unit>`, then re-checked before
each dependent write. Final desired bytes are computed before the plan so byte-identical no-ops are
omitted rather than classified as MODIFY.

### Step 4 - verify after authorized installation

After the workflow has instantiated the control plane under its declared authority, run the shipped
verification suite from the installed C2GRAPH target and review its report. Product implementation is
still separately goal-gated.


## Your first run

Pick **exactly one** door for a run. Do not combine or chain entry doors in the same run.

### Door A - existing repo, adoption ends with an implementation goal

```bash
pbcopy < SCAN-AND-ADOPT.prompt.md          # macOS
xclip -sel clip < SCAN-AND-ADOPT.prompt.md # Linux
Get-Content SCAN-AND-ADOPT.prompt.md | Set-Clipboard   # Windows PowerShell
```

Or tell the agent: `Read SCAN-AND-ADOPT.prompt.md and execute it from PHASE 0.` This path sets
`ENTRY_MODE=ADOPT_WITH_GOAL`; it inventories goal inheritance before verification and applies lineage
only in PHASE 8 after freshness is rechecked. It has 39 gates (`G0..G36` plus `G1b` and `G28b`).

### Door B - existing repo, workflow only; no active goal required

Choose this door only when the repository already contains project content and the requested outcome
is to install, repair, upgrade, or verify the C2GRAPH control plane **without** starting product
implementation. A missing goal, an empty goal ledger, or a ledger containing only terminal goals is
valid here.

If the agent can read the repository, the safest invocation is a direct instruction to read the
wrapper rather than manually reconstructing its rules:

```text
Read `C2GRAPH_SOURCE_ROOT/INSTALL-WORKFLOW-ONLY.prompt.md` in full, from its first
instruction through its final STOP condition. This request is strictly `ENTRY_MODE=WORKFLOW_ONLY`.
Do not run `SCAN-AND-ADOPT.prompt.md` as a standalone entry point, do not create or mutate any goal,
do not modify product behaviour, execute every delegated phase/gate/STOP rule, use only the
WORKFLOW_ONLY PHASE 8 finalization branch, report all seven required status fields, and stop without
starting product work.
```

If you prefer to paste the complete wrapper into a host that cannot read local files:

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

Do **not** replace that wrapper with a direct request to execute `SCAN-AND-ADOPT.prompt.md`. Direct
SCAN means `ENTRY_MODE=ADOPT_WITH_GOAL`; the workflow-only wrapper is what records the explicit
zero-goal/control-plane-only intent and constrains the delegated SCAN run.

Before its first write, the wrapper snapshots `.agent/goals/`: ledger existence plus SHA-256 for every
pre-existing regular goal file. It then classifies the state as `absent`, `empty`, `active`,
`terminal_only`, `inactive`, or `invalid`; sets `ENTRY_MODE=WORKFLOW_ONLY`; and runs the complete SCAN
pipeline. The mode changes only the permitted write scope and the PHASE 8 goal-finalization branch. It
does not skip scanning, the human decision gate, approved-tool handling, verification, blocker
handling, rule preservation, or any other applicable gate.

The final report must contain all seven fields:

```text
ENTRY_MODE       : WORKFLOW_ONLY
GOAL_STATE_BEFORE: absent | empty | active | terminal_only | inactive | invalid
GOAL_STATE_AFTER : absent | empty | active | terminal_only | inactive | invalid
GOAL_MUTATIONS   : 0
PRODUCT_CHANGES  : 0
WORKFLOW_STATUS  : installed | repaired | upgraded | verified
PRODUCT_READY    : no
```

A successful workflow-only run preserves the semantic goal state and every pre-existing goal-file
hash, reports zero goal mutations and zero product changes, and **stops**. It never auto-resumes an
active goal and never creates a maintenance/replacement goal just to make installation possible.

If the repository is empty/new, use Door C. If a requested edit changes product/application source,
business logic, migrations, product tests, or feature configuration, workflow-only must stop and the
work must move to a real goal-bound session. If the goal ledger is invalid, the installer may report
that condition but may not rewrite goal records to make the installation appear healthy.

### Door C - new or empty repo

```bash
pbcopy < BOOTSTRAP-NEW-REPO.prompt.md
```

Or tell the agent: `Read BOOTSTRAP-NEW-REPO.prompt.md and execute it from PHASE 0.` It has 35 gates
(`N0..N32` plus `N1b` and `N24b`).

### What happens, phase by phase

```text
PHASE 0   Recall prior failures, inventory the environment and tool availability, and
          perform no installation.
PHASE 1   Scan the existing repo in ultra detail, including tool call sites, tests/documents,
          discovery evidence, and the read-only goal-state/inheritance inventory.
PHASE 2   Measure discovery selectivity, then keep inert draft material in session/host evidence;
          PHASE 2 performs zero target persistence.
PHASE 3   STOP at the human decision gate until every required answer is recorded.
PHASE 4   Install/configure only tooling explicitly approved by the human.
PHASE 5   Instantiate the C2GRAPH control plane and preserve/migrate the existing rulebook
          according to the approved decisions; this is where normal repository writing begins.
PHASE 6   Run the complete existing-repo verification gate set and all shipped verifiers.
PHASE 7   Report evidence, blockers, gate results, and checkpoint state.
PHASE 8   Re-check goal-state freshness, then finalize by entry mode. ADOPT_WITH_GOAL may
          apply lineage and emit only after readiness; WORKFLOW_ONLY performs integrity/preservation
          checks, emits no goal, changes no product behaviour, reports its seven status fields, and stops.
```

---

## What you will be asked

Far less than you would expect, and never all at once.

Before anything is asked, the run prepares a full logical draft of your configuration. Through PHASE 3
the draft stays in session/host evidence and the target remains byte-identical; generic rollback
authority does not authorize an intermediate draft write. A draft file may be persisted only after
PHASE 3.W when its exact destination/action passes semantic scope, actual-write-set, path-scoped
authority, and freshness checks. Every value the repository proves is marked `DERIVED` and cited with a
`path:line`; every reversible template default
is `DEFAULTED`; only genuinely undecidable values become questions. Marking one that way still requires
the search that failed, so uncertainty cannot be used as an excuse to hand the work back to you.

What is left arrives in **PHASE 3**, and the shape of that gate matters as much as its content:

- **One question per message.** The run sends a question, stops, and waits. Eleven questions in one
  wall of text get skimmed rather than read, and a skimmed blocker gets an answer nobody actually
  thought about.
- **The queue length is fixed before the first question goes out**, and every question shows where you
  are, like `Question 3 of 9`. You always know how much is left.
- **Every answer is captured the moment it arrives.** Before PHASE 3.W it stays in
  session/host evidence. After PHASE 3.W it is persisted only when the exact answer destination is
  semantically required, listed in `ACTUAL_WRITE_SET`, and path-scoped authority/freshness are green. If a session
  dies at question six, the next run resumes at question seven without requiring an unauthorized write.
- **The queue can legitimately be empty.** When the draft settled everything, the run says so and
  keeps going. Inventing a question to look thorough is forbidden.

Each question carries a recommended default, the reason behind that recommendation, what happens if
you choose wrong, and a `[BLOCKER]` tag when the run cannot continue without it. The ceiling is
`question_queue_max` in `.agent/config.yml`, currently 25.

Example of what you will see:

```text
Q-03  Corpus partitions [BLOCKER]
      I found: web/, api/, shared/, legacy-php/, generated/
      (a) web, api, shared          - exclude legacy-php and generated
      (b) web, api, shared, legacy  - include legacy as its own partition
      (c) one single partition
      RECOMMEND (a): legacy-php has had no commit in 14 months and mixes into
      63% of search results for "user".

Q-TOOL-04  Two deploy paths, one purpose
      make deploy                    (Makefile L22)
      npx sst deploy --stage prod    (.github/workflows/deploy.yml L51)
      (a) Makefile is authoritative
      (b) CI workflow is authoritative
      RECOMMEND (b): CI ran 47 times in the last 90 days, the Makefile target did not.
```

You answer one question at a time, so a reply is usually a single word:

```text
b                                      answer this one question
use the recommendation                 take the recommended option and move on
use all recommendations for the rest   end the interview here
```

The third one is the escape hatch, and it is offered on the very first question so a long queue never
feels like a trap. Every remaining non-blocker takes its recommended option, every remaining blocker
is still asked because it has no default to fall back on, and the final report lists every value
settled this way so you can revisit it later.

In Bahasa Indonesia the same three replies are `b`, `pakai rekomendasi`, and
`pakai rekomendasi untuk sisanya`.

---

## What gets created

After your first run:

```bash
tree -L 2 -a .agent docs
```

```text
.agent/
  config.yml            your project profile
  tooling.yml           what tools exist + what claims are impossible without them
  toolmap.jsonl         what this repo ACTUALLY runs, with call-site citations
  lessons.jsonl         structured lessons, queryable
  requirements.jsonl    extracted requirements
  scopes/               per-domain rules, loaded only when that lane is active
  runbooks/             procedures, loaded only when that procedure runs
  runbooks/generated/   runbooks written from your real call sites
  state/                HANDOFF.md, CURSOR.json, discipline.jsonl
  goals/                emitted goal contracts
  graph/schema/         node and edge definitions + 30 worked examples
docs/
  graph-engineering/    20 numbered studies (00-19) explaining the design, plus decisions/references support files
  guides/               per-lane verified-behaviour guides, extracted from tests
```

**The following runtime artifacts are deliberately not shipped** and are born from measurement instead:
`tooling.yml`, `requirements.jsonl`, `toolmap.jsonl`, `golden.jsonl`, `_ledger.jsonl`, `HANDOFF.md`,
`handoff.jsonl`, `CURSOR.json`, `discipline.jsonl`, `goals.jsonl`, `GOAL-<nnn>.md`,
`generated/<tool>-<procedure>.md`, `<lane>-verified-behaviour.md`, `_conversion-ledger.md`.

Shipping them empty would be harmful: an empty file that parses cleanly passes gates and hides the
fact that the measurement never ran.

---

## Command cheat sheet

Things you will actually type, day to day.

### Start work

```text
Read AGENTS.md, then route this request: add rate limiting to the login endpoint.
Name the scope and runbook you load before you touch anything.
```

### Ask for evidence

```text
Cite file and line for that claim, or mark it not_verified.
```

### Before a long run

```text
Resolve `C2PY`, declare the resource cap, reserve continuity capacity, then start. For SCAN the
hard cap is 300 with a 12-call reserve (soft boundary 288); BOOTSTRAP is 200/12 (soft 188).
At the reserve boundary emit `C2GRAPH_RESUME_CAPSULE v1` and return `suspended_resumable`; do not
wait for hard exhaustion and do not report terminal `stopped` merely because budget is insufficient. Prompt 97 opens a new execution/resource epoch (`BUDGET_EPOCH + 1`, local `BUDGET_USED = 0`) while preserving cumulative usage only for audit, preventing an exhausted-budget resume loop.
```

### Force a checkpoint right now

```text
Run .agent/runbooks/context-compaction.md Procedure A. Budget: 8 tool calls.
Persist CURSOR.json last only when that exact post-3.W action is in ACTUAL_WRITE_SET,
path-scoped authorized, and fresh. Otherwise emit a complete PENDING_HANDOFF in
session/host evidence and write nothing under .agent/state/.
```

### Resume after compaction or a new session

```text
For a resource-suspended run use Prompt 97 with the newest durable handoff or pasted
`C2GRAPH_RESUME_CAPSULE v1`. Re-resolve `C2PY`, echo the capsule state, check only exact next-action
freshness/divergence, and continue. `.agent/state/**` is not mandatory when the transcript capsule survived.
```

### Ask for the goal block

```text
Run prompts/08-goal-emit.prompt.md. If the readiness gate fails, show me the
missing items instead of a goal.
```

### Check the router is still legal

```bash
wc -l AGENTS.md          # must be under router_max_lines (195)
grep -c '^| ' AGENTS.md  # routing table size
```

### Validate your JSONL by hand

```bash
for f in .agent/*.jsonl .agent/state/*.jsonl .agent/graph/schema/*.jsonl; do
  [ -s "$f" ] || continue
  n=0
  while IFS= read -r line; do
    n=$((n+1))
    printf '%s' "$line" | jq -e . >/dev/null 2>&1 || echo "INVALID $f:$n"
  done < "$f"
done
echo "jsonl check done"
```

### See how disciplined the run actually was

```bash
jq -r '.violation // empty' .agent/state/discipline.jsonl | sort | uniq -c | sort -rn
```

```text
   4 V03   claim without evidence
   2 V10   work repeated
   1 V05   gate green without output
```

### List tools nobody actually uses

```bash
jq -r 'select(.invocations | length == 0) | .name' .agent/toolmap.jsonl
```

### Find commands that exist only in documentation

```bash
jq -r 'select(any(.invocations[]; .kind == "docs_only")) | .name' .agent/toolmap.jsonl
```

---

## When your CLI compacts

This is the feature people feel first.

### The problem, concretely

```text
[context 94%]  ... compacting conversation ...

You:    continue
Agent:  Let me first explore the repository structure...
        [reads 30 files it already read]
        [re-runs the same 6 searches]
        [reaches a slightly different conclusion]
```

You just paid twice, and the second answer disagrees with the first.

### The fix - paste this at the start of a long session

```text
For the rest of this session, treat the conversation as a working surface, not storage.

Before any compaction, preserve a complete handoff. Persist .agent/state/HANDOFF.md only
when that exact post-3.W action is in ACTUAL_WRITE_SET, path-scoped authorized, and fresh;
otherwise emit PENDING_HANDOFF only in session/host evidence and do not create
.agent/state/**. Rehydrate from the valid
checkpoint surface before acting. Follow .agent/runbooks/context-compaction.md.

Checkpoint when ANY of these is true:
  A. the host announces compacting or summarizing
  B. context usage crosses 70%
  C. 25 steps have passed since the last checkpoint
  D. you cannot recall a fact you recorded earlier
  E. a session boundary occurs

Checkpoint budget: 8 tool calls maximum. Write CURSOR.json LAST only when its exact
post-3.W action is in ACTUAL_WRITE_SET, path-scoped authorized, and fresh; otherwise keep
the checkpoint as PENDING_HANDOFF outside target bytes.

After compaction: validate the newest continuity surface--durable CURSOR/HANDOFF when present,
otherwise `C2GRAPH_RESUME_CAPSULE v1` from transcript/external evidence--then emit the resume echo
BEFORE doing work. Pull only graph state needed by exact `NEXT_ACTION`. Report DIVERGENCE if the
rehydrated evidence contradicts the checkpoint; absence of `.agent/state/**` alone is not divergence.

Re-running discovery that the handoff already records is FORBIDDEN.
```

That block is also in the package as `prompts/98-compaction-handoff.prompt.md`, part **98A**.

### What you get back after compaction

Instead of the agent starting over, you get this and nothing else until you confirm:

```text
RESUMED FROM   HOF-2026-07-28-004
OBJECTIVE      Add rate limiting to auth endpoints
UNITS          7/12
ACTIVE UNIT    T-118 login endpoint middleware
HALF-DONE      middleware written, tests not yet run
NEXT ACTION    run pnpm test src/auth
FILES TOUCHED  6
GUARD METRIC   p95_login_ms: 142 (baseline 138)
BUDGET         118/300
DIVERGENCE     none
```

`DIVERGENCE` is the safety valve. If the written state contradicts what the agent believes, it says
so instead of silently picking one.

### Measure your own CLI first

```text
Run prompts/07-compaction-drill.prompt.md against this host.
```

Different hosts compact differently, and some do it **silently**. Run the drill once per host,
before you rely on the mechanism rather than after.

---

## The goal block

Almost every CLI and IDE now has `/goal`, a task field, or plan mode. Almost nobody fills it in,
because writing a good one by hand is tedious. So the agent asks *what next?* after every stage and
drifts a little each time.

**Goal-emitting paths write it for you.** At PHASE 8, adoption/bootstrap emits a filled-in block
only after readiness. `WORKFLOW_ONLY` deliberately emits no goal.

### What it looks like

```text
PROJECT      acme-web
GOAL-ID      GOAL-003
OBJECTIVE    Ship rate limiting on all authentication endpoints
DONE WHEN    all 4 auth endpoints return 429 past threshold, covered by tests,
             p95 latency not regressed beyond baseline + 5%

STAGES
  S1  Middleware
      scope     src/auth/middleware, shared/limiter
      output    limiter module + unit tests passing
      gate      pnpm test src/auth green, coverage not reduced
      rollback  commit tag pre-s1
  S2  Endpoint wiring
      scope     4 auth routes
      output    429 responses with Retry-After header
      gate      integration suite green
      rollback  commit tag pre-s2
  S3  Observability
      scope     metrics + dashboard
      output    rate_limit_hits counter emitting
      gate      metric visible in staging for 10 minutes
      rollback  feature flag RATE_LIMIT_METRICS off

GUARD METRIC p95_login_ms: baseline 138, must not regress beyond 145

ALWAYS       read AGENTS.md first; route before working; cite file+line;
             run the check before claiming done
NEVER        invent metrics; skip a gate; guess a human decision;
             re-run discovery recorded in HANDOFF.md
STOP AND ASK schema change needed; a mandatory gate remains red; a human/sensitive decision is unresolved
SUSPEND     budget/context/rate/host boundary -> capsule + suspended_resumable + Prompt 97

RESUME       newest durable handoff OR portable capsule; resource suspensions use Prompt 97
BUDGET       300 tool calls
STATUS LINE  emit PREFLIGHT at turn start and STATUS at turn end
```

### It refuses to emit a bad goal

Seven conditions must hold. If they do not, you get the missing list instead:

```text
READINESS GATE: FAILED (2 of 7)
  [x] every requirement has an id and a testable statement
  [x] zero requirements marked needs_confirmation
  [ ] acceptance criteria missing for REQ-009, REQ-014
  [x] stage boundaries defined and ordered
  [x] every stage has a gate with observable output
  [ ] no rollback unit named for stage S3
  [x] guard metric exists with a baseline

No goal emitted. A goal over an incomplete requirement set turns a guess into a
standing instruction.
```

### When the work is finished

A maintenance goal is optional and explicit, not an automatic side effect of completion. Emit one
only when ongoing goal-bound operational work genuinely needs it. `WORKFLOW_ONLY` on a repository
whose prior goal is completed must preserve that terminal goal and must not synthesize or reactivate a
maintenance goal.

---

## Reading the status lines

Every work turn opens and closes with one line each. Two lines, cheap enough to always emit - which
is the only reason they get emitted at all.

```text
PREFLIGHT | phase 5 | scope backend-api.md | runbook db-migration.md | partition api | mode RESUME
... work happens ...
STATUS    | stage S2 | gate green | units 7/12 | evidence 3 | budget 118/300 | next apply migration 0007
```

How to read them at a glance:

| Field | Good | Suspicious |
| --- | --- | --- |
| `scope` | a real filename | `none` on a domain task |
| `gate` | `green` with pasted output | `green` with nothing shown - that is V05 |
| `evidence` | goes up as claims are made | `0` next to confident prose - that is V03 |
| `budget` | rises steadily | jumps 40 in one turn - something re-scanned |
| `mode` | `RESUME` after compaction | `NEW` after compaction - that is V09 |

When discipline persistence is an exact semantically required post-PHASE-3.W action with path-scoped
authority and current freshness, violations are appended to `.agent/state/discipline.jsonl`; otherwise
they remain `PENDING_DISCIPLINE` in session evidence. Only unresolved blockers that actually prevent
the required next action count toward the stop threshold; pending observability rows do not.

---

## The problem it solves

| Symptom | Root cause | Handled by |
| --- | --- | --- |
| Rules pile into one giant file read on every request | invariants not separated from procedures | three-layer `AGENTS.md` |
| The agent is confident and wrong | no evidence contract | `evidence-contract.md`, CORE-05 |
| Discovery returns wide, mixed results | no corpus partitions | `graph-discovery.md`, CORE-18 |
| No way to prove the project moved forward | no guard metric | `prompts/01-measurable-loop.prompt.md`, CORE-20 |
| Runs become unresumable when a resource runs out | no continuity reserve/capsule | `docs/graph-engineering/11-complexity-budget.md`, `prompts/97-budget-resume.prompt.md`, CORE-24 |
| A swarm is used where one context was needed | architecture chosen without criteria | `docs/graph-engineering/10-architecture-selection.md` |
| Duplicate nodes merged wrongly | entity resolution without evidence | `entity-resolution.md` |
| The same lesson is relearned weekly | lessons unstructured, unqueryable | `.agent/lessons.jsonl`, `prompts/02-lessons-corpus.prompt.md` |
| After compaction the agent redoes finished work | state lived only in the conversation | `context-compaction.md`, CORE-25 |
| Implementation asks what next at every stage | the objective was a message, not a contract | `goal-contract.md`, CORE-26 |
| The report reads better than the run went | compliance never observable per turn | `workflow-discipline.md`, CORE-27 |
| The agent invents a procedure for your own tool | real usage never captured from call sites | `tool-usage-capture.md`, CORE-28 |
| Docs and tests disagree, agent believes the prose | no ordering between honest and aspirational sources | `test-to-workflow.md`, CORE-29 |
| A mobile release ships with no way back | web assumptions on an irreversible artifact | 3 mobile runbooks, CORE-31 |

---

## What AGENTS.md actually does

C2GRAPH treats `AGENTS.md` as the always-loaded router on hosts that honor the root-instruction convention.
The current English router is 195 lines and the Indonesian router 194, both within the configured limit of
195. It contains the rules that apply before task classification plus a 42-row routing table.

That is the whole design. The router states an obligation and points; it never explains. Every
explanation was pushed down into a scope or a runbook that loads only when it is actually needed.

```text
LAYER 1  AGENTS.md              host-loaded root router    195 EN / 194 ID lines
LAYER 2  .agent/scopes/*.md     loaded when a lane is active
LAYER 3  .agent/runbooks/*.md   loaded when a procedure runs
```

The routing table is written in **symptoms**, not module names, because that is how people actually
speak:

```text
| too complicated, simplify, convoluted      | runbooks/simplification-ladder.md |
| production broken, need to go back         | runbooks/incident-rollback.md     |
| compacting, context full, lost context     | runbooks/context-compaction.md    |
```

Nobody types "I require the simplification ladder". Routing only works if the trigger matches the
words someone uses at the moment they have the problem.

---

## Why it costs less to run

This is the part that shows up on your invoice, and it is the easiest benefit to verify.

**A big rulebook is charged on every request.** A 1200-line monolithic rulebook means roughly 12-16k
tokens ride along on every request, every lane, every subagent. The current router is 195 lines in
both English and Indonesian, exactly at the 195-line fence; the rest loads only when relevant. The
1.5-2k token figure is only a rough historical scale estimate and must be measured on the target
tokenizer before it is treated as a cost claim.

**The bigger win is prompt caching.** Because Layer 1 is the same bytes at the start of every
request, the prefix is **stable** - and a stable prefix is what prompt caching needs in order to hit
at all. A rulebook that changes shape from request to request cannot be cached, so you pay full
price every time. This is designed in, not accidental: the scope template literally opens with

```text
<!-- LAYER 2: SCOPE. Loaded on demand. Not part of the cacheable prefix. -->
```

The volatile material is deliberately kept out of the cached region.

**The router cannot quietly grow.** `router_max_lines` lives in `.agent/config.yml` and gate `G2`
fails the run if `AGENTS.md` exceeds it. The cheapest way to break the economics of this template is
to let that one file drift upward, so it is enforced rather than trusted.

**Discovery is not paid for twice.** Before compaction, the agent preserves complete logical
continuity in transcript/session evidence. Durable `.agent/state/HANDOFF.md` is persisted only when its
exact post-PHASE-3.W action is semantically required, present in `ACTUAL_WRITE_SET`, path-scoped
authorized, and fresh; otherwise continuity remains `PENDING_HANDOFF` plus the portable resume capsule.
Re-running discovery recorded by the valid checkpoint is forbidden.

**One honesty note, because the template insists on it.** `docs/graph-engineering/04-context-budget.md`
separates what you may claim from what you must measure first:

| May be claimed | Requires instrumentation first |
| --- | --- |
| token tax per request dropped N% | cache hit rate went up |
| the router is N lines | answer quality went up |

Token arithmetic is arithmetic. Cache-hit rate and answer quality are empirical claims about *your*
provider and *your* traffic, so the template asks you to measure them in your own repo rather than
inherit a number from a README. The structure that makes caching possible ships with the template;
the percentage is yours to measure.

---

## Where this came from

This is not a rules file someone wrote in an afternoon. It is the output of a research loop that is
itself shipped in the package, so you can re-run it on your own repository instead of taking the
result on faith.

The method is **audit first, change second**. `docs/graph-engineering/00-audit.md` is explicitly
read-only with respect to the repo, and carries this rule:

> This document is forbidden from containing recommendations that have already been executed. An
> audit only measures. Execution lives in the other prompts.

The baseline audit measures what is actually in force before anything is touched: rule inventory and
tokens per request, a duplication matrix showing which rules say the same thing in more than one
place, gates that are written down but never executed, and the retrieval quality of the existing
corpus.

From there the investigation runs as **20 numbered studies (00-19) driven by 12 reusable prompts**:

| Study | The question it answers |
| --- | --- |
| 00 baseline audit | what rules are in force, and what do they cost per request |
| 01 measurable loop | which metric tells us this got better, and what is its baseline |
| 02 lessons | which corrections keep recurring, and do they decay |
| 03 graph plane | what entities and relations does this work actually have |
| 04 context budget | what moves out of the router, and what does that save |
| 05 retrieval eval | does discovery find the right file, scored against a golden set |
| 07 system inventory | which tools exist here, and what can be claimed without them |
| 11 complexity budget | what can the run spend while preserving enough capacity for a resumable handoff |
| 12 compaction handoff | what survives a context reset |
| 17 workflow discipline | which violations recur often enough to warrant a hard stop |

Each study is paired with a prompt (`prompts/00-baseline-audit.prompt.md` and friends) so the finding
is reproducible rather than anecdotal. The rules that ended up in `AGENTS.md` are the ones that
survived that process - the ones that apply everywhere, before the domain is known. Everything else
was pushed down a layer, which is why the router remains below its 195-line fence instead of growing into an 800-line rulebook.

The automation follows the same discipline. Three entry prompts cover three distinct states:
`SCAN-AND-ADOPT.prompt.md` for existing-repo adoption that ends goal-bound,
`INSTALL-WORKFLOW-ONLY.prompt.md` for an existing repo whose control plane must be installed or
upgraded without touching goals or product behaviour, and `BOOTSTRAP-NEW-REPO.prompt.md` for an empty
repo. SCAN has 39 gates (`G0..G36` plus `G1b` and `G28b`); BOOTSTRAP has 35 (`N0..N32` plus `N1b` and
`N24b`). Workflow-only reuses the SCAN gates with the G35 no-goal preservation branch. A gate either
produced its expected output or it did not; there is no "looks fine" state anywhere in the flow.

---

## Pre-public refutation evidence folded into v1.0.0

All engineering snapshots described during development remain pre-public evidence for the **same pre-release engineering baseline that became public v1.0.0 on 12 August 2026**.
They are not prior public versions. The release evidence is intentionally based on the final live tree rather
than on historical checker counts from intermediate snapshots.

The exact v2 tree was replayed at child-process granularity so a host timeout could not be relabeled as PASS.
Twenty-seven registered child checks per language, including the bounded-read self-test, completed successfully in
the process-isolated matrix; the spawn-heavy `selftest/run.py` was then executed separately in both editions and
returned `VIOLATIONS (0)`. Together those known exit states cover all **28/28 registered checks in English and
28/28 in Indonesian**. A monolithic aggregate attempt that exceeded the host execution window remains recorded as
incomplete rather than being promoted to evidence.

Bounded Read v2 was then attacked in three repeated process/deterministic reviewer rounds against the exact optimized
EN/ID trees. Each round ran 40 differentiated jobs (20 roles in each language); all three completed **40/40**, for
**120/120 process jobs**. The roles cover direct source-offset continuation, source-generation drift, profile
mismatch, the sequential-window strategy boundary, CRLF/UTF-8 boundaries, mega-lines, EOF/no-final-newline,
binary/path/symlink outcomes, hard ceilings, and native reader verifier/self-test surfaces. They are **not 120
independent LLM agents**.

A separate fixed-seed refutation completed **3,000/3,000 exact reconstructions**, **3,000/3,000 valid arbitrary
legacy starts**, **400/400 cross-window mutation cases with stale-generation rejection**, **500/500 stable
concurrent reads**, and **3,000 leaf-swap attempts with zero observed outside-root bytes**. This is finite local
filesystem/process evidence, not formal proof against every kernel, filesystem, or same-privilege writer.

Performance was remeasured on the optimized v2 code instead of copied from an earlier reader snapshot. The
plain-vs-C2GRAPH experiment used three rounds, five timing repetitions and two `tracemalloc` peak repetitions per
reader/fixture/round. Results are deliberately mixed: plain whole-file Python is cheaper for small ordinary files;
C2GRAPH bounds first-view evidence and lowers Python peak allocation on sufficiently large/hostile shapes in this
harness. The 131,072-byte default is a **normalized evidence-content ceiling**, not a bound on serialized JSON/stdout,
process RSS, model tokens, or total source traversal. Generated v2 continuations seek from their recorded source
offset; legacy arbitrary `(line,char)` starts may still scan once to locate that first exact offset.

The census scanner was also remeasured on deterministic synthetic repositories using three sequential warm-cache
rounds in one local container. Median elapsed wall time was **1.362 s at 1,014 files, 2.458 s at 10,014 files, and
6.944 s at 50,014 files**; all nine invocations returned exit 0 and reported the expected file count. These three
measurements do not establish a universal linear/superlinear growth model or a fixed 10,000-file split threshold.
Use `--max-files` as a declared work budget and partition according to the selected target and host constraints.

The final release-facing sentence/statement inventory covers the EN/ID repository, external docs, flowchart Markdown/DOT labels, and both paper DOCX files. Every conservative review flag receives a manual class and evidence disposition; the release criterion is **0 unresolved items**. Exact inventory and review-flag counts are recorded only in supplementary audit evidence so that the counted release-facing surfaces do not contain a self-referential count that changes the inventory being measured. The classifier is an audit aid rather than semantic proof. Final documentation and diagram closure metrics are likewise recorded in the supplementary release evidence after the prose freeze.

The audit process also retained counterexamples against its own tooling. Stale-root harnesses, a wrong scanner JSON
field assumption, bytecode contamination, and behavioral-oracle mistakes were classified as **audit-harness
findings**, not product defects. Accepted design limitations are recorded separately from repaired defects. The
release claim therefore stays narrow: **after repair, no defect remained observed in the enumerated final test space**.
That statement does not imply universal bug absence, production token savings, model-quality improvement, or
formal filesystem security.

## Tuning it later, in one place

Every operational threshold enforced by configuration lives in `.agent/config.yml`; documentation should name the
setting rather than duplicate its value unless a measured release value is explicitly being reported. Raise `router_max_lines` and the check, the router's own
header note, and the layer diagram all follow. Lower `slice_specificity_min` and the discovery ladder
and the adoption gate follow with it. The two interview ceilings, `question_queue_max` and
`seed_interview_max`, work the same way.

That is a deliberate constraint rather than a convenience. A threshold written twice is a threshold
that will eventually disagree with itself, and the version you happen to read is decided by which file
you opened. The checklist carries the rule in both editions now: a number in the documentation either
matches the configuration or names the key it came from.

---

## After the goal is done

When every implementation stage is complete, mark that implementation goal terminal. If the team
chooses to keep an ongoing operational objective, create a separate **MAINTENANCE goal** explicitly;
do not reactivate the completed implementation goal and do not create maintenance merely to satisfy
workflow installation. A workflow-only upgrade may legally leave the repository with zero active goals.

When ongoing maintenance is actually desired, a goal-bound maintenance contract can look like this:

```text
/goal
PROJECT : <name>
GOAL-ID : GOAL-<nnn>  (MAINTENANCE)
OBJECTIVE:
  Keep <system> passing every gate it passed at handover, and keep every claim evidence-backed.

STANDING GATES (before any change is declared done):
  - <lint command> exits 0
  - <test command> exits 0, zero new failures
  - guard metrics no worse than: <name>=<value>, <name>=<value>
  - per-route post-deploy QA recorded for every affected route

WEEKLY HYGIENE:
  - router still under router_max_lines
  - zero orphan scope or runbook files
  - weekly average sliceSpecificity recorded
  - contradicting lessons resolved
  - recurring ToolingGaps raised as tickets

ALWAYS: <the ALWAYS list from the implementation goal>
NEVER : <the NEVER list from the implementation goal>
RESUME: use the newest validated durable handoff/cursor OR `C2GRAPH_RESUME_CAPSULE v1`;
absence of `.agent/state/**` is not a blocker. Resume through Prompt 97 from exact `NEXT_ACTION`.
```

Three more prompts you will want on the day after completion:

**Run the weekly hygiene pass.**

```text
Run the weekly hygiene pass from docs/graph-engineering/06-operations.md.
Report each item as a number with the command that produced it, not as a sentence.
If the router is within 10 lines of router_max_lines, say so and propose what moves down a layer.
```

**Close the loop on what the run taught you.**

```text
Review .agent/lessons.jsonl for this milestone.
List corrections that recurred at least twice, and for each one say whether it belongs in a scope,
a runbook, or nowhere. Cite file+line for anything you propose changing. Do not edit yet.
```

**When the objective itself changes.** Editing a goal in place is forbidden - goals are superseded,
not rewritten, so the old one stays addressable:

```text
The objective has changed: <what changed>.
Supersede the active goal per .agent/runbooks/goal-contract.md: write the next GOAL-<nnn+1> in full
(not a diff), add SUPERSEDES to its header, mark the old one superseded in goals.jsonl, and re-emit
the new block so I can replace the copy pasted into my host.
```

That last instruction exists because of a specific failure: a goal edited quietly in a file while a
stale copy still sits in the host's memory is worse than no goal at all, since nobody knows to
re-paste it.

---

## Where this sits

It is worth being exact about what layer this occupies, because the neighbouring layers are moving
fast and two of them are being absorbed by platforms.

**Below it: the harness.** Approval modes, resumable sessions, subagents, sandboxing. This is vendor
territory and it is consolidating quickly. Nothing here tries to compete with it. Where the package
touches durability, it does so in a form the harness cannot supply: not a transcript that can be
replayed, but a record of which findings already carry a verdict and are therefore forbidden to
rediscover. A vendor will ship the mechanical checkpoint. Nobody will ship your policy.

**Beside it: the rules file.** `AGENTS.md` is a widely adopted convention now, and the convention
specifies a filename and a habit, not a structure. Published guidance for it is largely about setup
commands, code style and test commands - a README addressed to an agent. That is genuinely useful and
it is also the whole of it. The convention has reach and no depth. What this package adds sits
entirely above that line: routing instead of a single file, scopes and runbooks instead of prose,
gates instead of intentions, and identifiers that outlive the file they were written in. It is a
profile on top of the convention, not a competitor to it.

**Above it: evidence that someone else can audit.** This is the part that gets undersold, so it is
stated plainly here. A run under this template already emits most of what an auditor asks for when
AI-generated code reaches a regulated review: which specification was in force, what the task was,
which tests passed, who accepted the result, and the order in which all of that happened. It emits it
as a side effect of the work rather than as a separate compliance exercise, which is the only way such
a record ever stays current.

What it does **not** emit today, stated so nobody has to discover it during an assessment:

| Field an audit trail usually wants | Present? |
| --- | --- |
| specification in force, with a permanent identifier | yes, the goal contract and `supersedes` |
| task description and plan | yes, as graph nodes |
| tests that passed, with pasted output | yes, and a claim without it is a violation |
| order of events, tamper-evident by append | yes, the ledger and the handoff log |
| the human who accepted the result | partly, as a confirmation step, not as a field |
| the model and provider that produced the change | no |
| a signature over the record | no |
| a mapping from each rule to a named control | no |

Those three gaps are small pieces of work and they are the difference between an internal discipline
and an artifact someone outside your team can read. They are listed here rather than in a roadmap
because a roadmap is a promise and this is a description.

**And a scope statement, since it is the honest way to close a positioning section.** This is a
specification with a small amount of shipped code, not a product. Nineteen validators check the
template. Twenty-three script contracts describe an interface and stop, because a template that
shipped Bash would be useless in a Windows Go repository. The graph needs a store you provide. The
work of turning the contracts into checks that run in your CI is yours, and it is the largest cost of
adopting this.

---

## The case against adopting it

A release post that only argues for itself teaches your agent to write reports the same way. So here
is the strongest honest case against this package, in the order the arguments actually bite.

**Part of it is on a commoditisation path.** Coding harnesses are now competing on exactly the ground
the durability layer covers: resumable work, approval modes, state that survives a session. If your
harness ships a good enough version of that, three runbooks here lose their reason to exist. The
semantic half - which discovery already has a verdict, which claim is unproven - is not on anyone's
roadmap, but the mechanical half plausibly is.

**The adoption cost is real and front-loaded.** 174 files, nine phases, thirty-nine gates, around
twenty-five questions, and 211 configuration keys. The realistic failure mode is not rejection, it is
partial adoption: a team keeps the thin router because it obviously saves tokens, and quietly drops
the gates, which is where the whole value is. If you are going to take half of it, take the gates and
leave the router.

**The evidence behind it is narrow.** It has been run daily against two production applications by one
author on one operating system with one harness. That is more field use than most templates of this
kind can claim, and it is still a sample of one team. There is no controlled comparison anywhere
behind the claim that it makes work better, and by the package's own honesty rule that claim therefore
should not be made. Token arithmetic is arithmetic; everything else is a hypothesis you test in your
own repository.

**Its own quality gate is not yet measured.** The golden retrieval set ships as five examples. Until
you replace them with real questions about your own code, the retrieval score is a target and not a
measurement, which means the single number that would prove discovery got better is the one number
nobody has.

**The validators check the template, not your repository.** `verify/all.py` proves the package is
internally consistent. It says nothing about whether your agent obeyed a rule yesterday. The
discipline log is what measures that, and the log only exists if the run writes it.

**It cannot force anything.** There is no runtime, no hook, no interceptor. A model that decides to
skip a step will skip it. What the package does is make that visible on the turn it happens, countable
in a log, and blocking on the third repeat. If you were hoping for enforcement rather than visibility,
this is not that.

**And it is subject to the disease it treats.** The whole argument rests on the router staying small.
A rule set that grows every time someone has a good idea becomes the 800-line file it replaced, one
sensible addition at a time. The line limit and gate `G2` exist for this reason, and they are the
first thing to check on any fork, including this one.

## Round 10: the function census

A fleet of fifteen auditors was pointed at both editions at once, each examining a different
dimension of the surface rather than re-running the same suite: script functions, the shared library,
config-key reachability, invariant citation, runbook and scope orphans, schema-to-script field drift,
phase and gate topology, chapter reachability, loop-metric automation, exit-code contracts, flag
contracts, check registration, edition parity, and a live execution of every read-only script.

It started at 72 findings and finished at 14, all low. The reduction was two-thirds correction of the
auditors themselves - a runner is not one of its own checks, a config key read through `thresholds()`
is not a record field, and a project descriptor waiting to be filled in is not dead weight - and
one-third real repairs:

| Defect | Why it mattered |
| --- | --- |
| `lessons/decay.py` measured age from `reconfirmed` while the schema declares `last_confirmed` | a lesson renewed yesterday was still reported stale, so re-confirmation quietly did nothing |
| Two ageing clocks, 30 and 60 days, with nothing saying they were different things | readers could not tell a reminder from a demotion |
| `install/preflight.py` did not set `dont_write_bytecode` | the one script that runs before installation could leave bytecode behind |
| Six config keys declared but never read or cited anywhere | a number nobody reads is a number nobody maintains |

The fourteen remaining findings are all the same shape: seven scopes carry no threshold table. That
is a deliberate variance, recorded rather than silenced - a scope carries numbers only when it has
numbers of its own.

That paragraph records the historical pre-release Round-10 surface rather than the final `v1.0.0` tree. The
self-measured v1.0.0 release package is larger: 43 Python scripts, 211 config keys, 38 invariants, 35 runbook files
(34 procedures plus the template), 20 numbered graph-engineering studies plus support files, and 28 registered checks.
`verify/cli.py` currently reconciles 48 documented and implemented flags. Current numbers come from
`verify/counts.py`; the historical function census is not used as a release claim.

---

## Provenance: what the ledger records, and what it cannot prove

A decision log that says only *what* was decided is worth little at audit time. The question asked
by every framework that will matter in 2026 - EU AI Act Article 12, SOC 2 change management, PCI DSS
4.0 requirement 6 - is narrower: *which* model proposed this, *which* human accepted it, and can you
show the record was not edited afterwards.

The `v1.0.0` baseline answers those three questions and refuses to pretend it answers a fourth.

`.agent/provenance.schema.md` fixes the record shape. Fourteen fields, each one documented, each one
covered by the digest: `id`, `record`, `producedAt`, `runId`, `phase`, `actor`, `model`, `provider`,
`acceptedBy`, `sourcePath`, `sourceLines`, `quote`, `status`, `prevDigest`. The three fields that were added during pre-release hardening and are intended for the first public release are: **`model` and `provider`** name the system that
proposed the decision, and **`acceptedBy`** names the person who took responsibility for it. A
record in `accepted` or `superseded` state with an empty `acceptedBy` is a violation, not a warning.

`scripts/provenance/export.py` turns that ledger into W3C PROV-O:

| Ledger concept | PROV-O node |
| --- | --- |
| a decision record | `c2:record/<id>` typed `prov:Entity` |
| the run that produced it | `c2:run/<runId>` typed `prov:Activity` |
| the model that proposed it | `c2:agent/<provider>/<model>` typed `prov:SoftwareAgent` |
| the human who accepted it | `c2:person/<acceptedBy>` typed `prov:Person`, reached through `prov:qualifiedAttribution` with role `c2:role/acceptor` |

The exporter is a dry run by default: it prints what it would emit and writes nothing. `--write`
emits the JSON-LD, `--sign` writes a SHA-256 manifest beside it, `--verify` recomputes the chain,
and `--selftest` runs the whole thing against the worked example in the schema page so the contract
is exercised even in a repository with no decisions yet. `--out` refuses any path outside the
repository root and exits 2.

Each record carries the digest of the one before it, so editing an old record invalidates every
record after it. `scripts/verify/provenance.py` is registered in `scripts/verify/all.py` and tampers
with a copy of the ledger on purpose during its own run: a checker that never sees a failure is not
evidence that failures are caught.

**What this does not give you.** The digest chain proves *integrity*, not *authorship*. It shows a
record was not altered after it was written; it does not show who wrote it, and a chain whose owner
can rewrite the whole file can be recomputed end to end. `--sign` produces a SHA-256 manifest, which
is a checksum, not a signature - there is no key, no identity, and no third party. Key-based signing
and an external timestamp are the honest next step, and they are not in this candidate. Nothing here
prevents a person from typing someone else's name into `acceptedBy`; the field records an
attribution, it does not authenticate one.

The shipped schema example uses `c2graph` as a placeholder acceptor so that the archive carries no
real person's name. It is a placeholder, not a pattern: on a real record, `acceptedBy` is the human who
took responsibility for the decision.

Two gates carry this into the run itself: **G29** for an existing repository and **N25** for a new
one. Both fail when a decision reaches `accepted` without `model`, `provider` and `acceptedBy`, and
both are counted in the roll-up rather than trusted.

### Two more planes: control mapping and rule preservation

The ledger answers *who decided this*. Two later rounds answer the two questions that followed it.

`.agent/compliance-map.md` maps every core rule and every evidence producing gate to a control in SOC 2,
PCI DSS 4.0 and the EU AI Act, names the file that would be shown as evidence, and states in words that a
map is not a certification. `scripts/verify/compliance.py` checks the shape, not the judgement: the rule
exists, the control identifier looks like something the framework actually publishes, the evidence file is
present, and the caveat has not been quietly deleted. Two core rules stay deliberately unmapped, because a
single citation for prompt injection or secret handling would mislead more than it helps.

`.agent/core-baseline.md` and `.agent/rule-registry.md` close a hole that only appears on adoption day. When
an agent scans a repository that already has its own rulebook, the tempting move is to regenerate the router
from what the scan found. What the scan cannot find is every rule whose situation has not happened yet: the
named rollback unit, the guard metric with a baseline, the handoff written before compaction. Those rules
survive precisely because nothing has gone wrong yet, and they are the first to be dropped. The baseline is
frozen and hashed before the first adjustment, the registry gives each rule a disposition, and
`scripts/verify/preserve.py` refuses a router that silently loses one. A rule may be relocated to a file that
exists and mentions it, or rejected with a reason and a named human approver, and nothing else.

Be clear about what this buys: it makes a lost rule **detectable**, not impossible. It cannot tell whether a
rule that is still present is being followed.

Gates `G30` and `G31` carry both planes into an existing repository, `N26` and `N27` into a new one.
Six configuration keys control it, and, as with every other number in the template, the document
that states one must name the key it came from:

```yaml
provenance_export_enabled: true
provenance_required_fields_enforced: true
provenance_digest_algorithm: sha256
provenance_signature_required: false
provenance_actor_unknown_allowed: false
provenance_example_min: 3
```

## Known limits

Here is what this template does **not** do. It is listed as plainly as the rest, because a document
that only advertises its strengths quietly teaches your agent to do the same thing.

- **30 validators/gates ship as runnable Python; the project-specific interfaces later in
  `scripts/README.md` remain contracts, not generic implementations.** The tree contains 43 Python
  files total: 30 validators/gates, four scanners, one exporter, one interview helper, one budget
  classifier, one additional resume-capsule utility, one shared library, and one self-test harness. The contracts are intentionally left to `{{PRIMARY_LANG}}`,
  because a template-level implementation would encode runtime assumptions the target repo may not
  share.
- **Compaction detection is best-effort on silent hosts.** Class D is a heuristic. Run
  `prompts/07-compaction-drill.prompt.md`.
- **`toolmap_generate_max` caps generation at 40 procedures.** Large monorepos need a second pass.
- **Store rules change.** `mobile-store-submission.md` encodes the shape of the process, not the
  current contents of either store's policy.
- **The golden set ships with only 5 examples.** Treat the retrieval score as meaningless until you
  swap them for real questions about your own repo.
- **Rule identifiers are permanent, and retired numbers stay retired.** If you fork and renumber,
  keep the old numbers as aliases or every cross-reference in the corpus rots silently.

---

## Design notes

**Why the checkpoint budget is 8 tool calls.** A checkpoint that is expensive gets skipped exactly
when it is most needed - under context pressure. Cheapness is the feature.

**Why generated runbooks are not authority.** They say *this appears to be what the repo does*. Only
a human can say *this is what the repo is allowed to do*. The gap between those two sentences is
where incidents live.

**Why an empty result is recorded.** An empty `failure_modes` array, a tool with zero invocations, an
untested device slot - each is a finding. Omission reads as "fine". Explicit emptiness reads as
"nobody has checked", which is the truth.

**Why V12 exists.** A report smoother than the run it describes is a violation. The smoothing hides
the place where the process actually broke, and that is the only part worth reading.

**Why `fallbackRate` has no target.** Give a number a target and it gets optimized. Optimizing
fallback rate means using a bad slice rather than dropping a rung - the opposite of what the ladder
is for. Some numbers are for reading, not for hitting.

---

## Feedback

The rule set is meant to be argued with. Most useful reports, in order:

1. A gate that fails on a clean install of the template itself.
2. A rule ambiguous enough that two readers implement it differently.
3. A host whose compaction behaviour none of the five detection classes catch.
4. A repo type the plan-to-docs matrix has no row for.

When you report something, cite `file:line`. The template holds itself to that rule, and so does its
issue tracker.

---

## Licence

Use it, fork it, strip it down. If you delete something, delete it **as a set** - half-configured
subsystems fail worse than absent ones, which is exactly what G22 checks for mobile.
