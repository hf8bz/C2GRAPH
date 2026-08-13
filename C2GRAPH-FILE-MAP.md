# C2GRAPH — Current File Map

> Version scope: C2GRAPH `v1.1.0`, Version 1 release, adapter-integrated source: **201 files per EN/ID edition** plus the separate **17-file documentation bundle**.

The EN and ID editions have the same relative-path set. Localized prose differs; executable adapter and verification code is checked for parity.

| Relative path | Purpose |
| --- | --- |
| `.agent/compliance-map.md` | every core rule and evidence gate mapped to SOC 2, PCI DSS 4.0 and EU AI Act controls, with the evidence file for each and a plain statement that a map is not a certification |
| `.agent/config.yml` | project: |
| `.agent/core-baseline.md` | the frozen list of rule identifiers the template shipped, written once at install time and hashed, so a later loss is arithmetic rather than opinion |
| `.agent/counts.generated.json` | the measured snapshot, written by `verify/counts.py --write` |
| `.agent/distribution-files.txt` | the authoritative shipped-path manifest used for distribution-count and parity checks |
| `.agent/draft/.gitkeep` | keeps the draft directory in version control |
| `.agent/failures.jsonl` | Append-only failure memory after write authority; pre-authority failures stay `PENDING_FAILURE` in session evidence. |
| `.agent/failures.schema.md` | Schema and retry policy for failure-memory records. |
| `.agent/goals.schema.md` | A goal is the objective a host holds across turns and across compactions - the /goal command, a |
| `.agent/goals/.gitkeep` | Emitted goal contracts live here. Written by .agent/runbooks/goal-contract.md, |
| `.agent/graph/.gitkeep` | Shipped C2GRAPH repository artifact. |
| `.agent/graph/schema/edges.md` | The six rows above are emitted by example.jsonl and referenced by the compaction, |
| `.agent/graph/schema/example.jsonl` | {"type":"Objective","id":"OBJ-001","title":"Checkout is usable at every breakpoint","owner":"team-web","created":"2026-01-05"} |
| `.agent/graph/schema/nodes.md` | This graph is not a code graph. It is a graph of work: it connects objectives, plans, |
| `.agent/handoff.schema.md` | Three artifacts, one purpose: make the work state survive a context compaction, a new session, or a |
| `.agent/lessons.jsonl` | Shipped configuration, schema, ledger example or generated measurement surface. |
| `.agent/lessons.schema.md` | One JSON object per line in .agent/lessons.jsonl. |
| `.agent/model-adapter.schema.md` | Provider-neutral capability/admission schema; explicitly keeps cache evidence non-authoritative. |
| `.agent/prefix.lock` | Digest lock for the stable router skeleton: rule IDs, headings, and bullet count. |
| `.agent/provenance.schema.md` | the fourteen fields of a decision record, the digest rule, and the worked three-record example the exporter self-tests against |
| `.agent/requirements.schema.md` | One JSON object per line in .agent/requirements.jsonl. Used mainly by new repos, but also |
| `.agent/resume-capsule.schema.md` | Portable target-independent checkpoint schema for resource suspension and CLI/session restart. |
| `.agent/routes.yaml` | Machine-readable routing table mirrored against the Markdown router. |
| `.agent/rule-registry.md` | one row per shipped rule saying where it lives now: kept, absorbed into a scope, moved to a runbook, rejected with a named human approver, or added by adoption |
| `.agent/runbooks/_TEMPLATE.md` | 1. |
| `.agent/runbooks/agent-dispatch.md` | When considering delegating part of the work to a subagent. |
| `.agent/runbooks/batch-protocol.md` | The work touches more than 5 files (batch_trigger_files in .agent/config.yml), or |
| `.agent/runbooks/blocker-handling.md` | Seven blocker classes, continuation decisions, and stop/fail rules. |
| `.agent/runbooks/bounded-read.md` | Operational read/recovery contract. |
| `.agent/runbooks/browser-tooling-recovery.md` | When a browser tool, a driver, or its integration fails and it is not yet clear whether the cause |
| `.agent/runbooks/budget-resume.md` | Budget/context/rate/host-session suspension contract with reserved handoff capacity and portable resume capsule. |
| `.agent/runbooks/context-compaction.md` | Compaction is the moment a CLI or IDE replaces part of the conversation with a shorter summary so |
| `.agent/runbooks/coverage-backfill.md` | Completeness census and human-approved backfill procedure (CORE-37). |
| `.agent/runbooks/db-migration.md` | Every schema change: tables, columns, indexes, relations, and data backfills. |
| `.agent/runbooks/discovery-cache.md` | Cross-session discovery cache and invalidation rules (CORE-36). |
| `.agent/runbooks/edit-tooling.md` | When changing many files mechanically: renames, signature changes, call-pattern replacements. |
| `.agent/runbooks/entity-resolution.md` | Entity resolution is the process of collapsing several mentions into one canonical ID: |
| `.agent/runbooks/evidence-contract.md` | Every time a claim, a finding, or an audit report is assembled, or something is declared verified. |
| `.agent/runbooks/generated/.gitkeep` | Generated procedure runbooks land here, one file per procedure, named |
| `.agent/runbooks/goal-contract.md` | Most coding CLIs and IDEs now expose a persistent-objective feature: /goal, or a task, mission, |
| `.agent/runbooks/goal-inheritance.md` | Read-only goal inventory before verification; lineage application only on goal-emitting PHASE 8 paths (CORE-38). |
| `.agent/runbooks/graph-discovery.md` | This answers one question: how to find the right place in the repo, with evidence, without |
| `.agent/runbooks/incident-rollback.md` | When production is broken, or when the last change has to be undone. |
| `.agent/runbooks/mobile-build-release.md` | Mobile differs from web in one way that changes every procedure downstream: a shipped build cannot |
| `.agent/runbooks/mobile-device-qa.md` | On the web, one browser at one width is a weak claim but a comprehensible one. On mobile it is |
| `.agent/runbooks/mobile-store-submission.md` | Submission is where mobile stops being an engineering activity. The gatekeeper is a review process |
| `.agent/runbooks/model-adapter.md` | Operational host-adapter admission, fail-closed runtime/cache and debugging procedure. |
| `.agent/runbooks/monorepo-index-placement.md` | where a code index belongs when the repository holds several applications (CORE-34) |
| `.agent/runbooks/plan-to-docs.md` | A plan that lives only in a conversation is lost when the conversation is compacted, and it is |
| `.agent/runbooks/post-deploy-qa.md` | After every deploy to an environment other people use. |
| `.agent/runbooks/preflight-recall.md` | Failure recall before repeating an action (CORE-35). |
| `.agent/runbooks/propagation.md` | Every time a human corrects the agent's work. |
| `.agent/runbooks/ratchet-loop.md` | A ratchet loop keeps only the changes that measurably improve a number and reverts everything |
| `.agent/runbooks/rule-preservation.md` | the procedure for merging an existing rulebook into the router without losing a rule, including the identifier collision trap |
| `.agent/runbooks/simplification-ladder.md` | Used before adding a new abstraction, layer, or dependency. |
| `.agent/runbooks/system-inventory.md` | A tool inventory is not administrative record-keeping. It determines claim limits. An absent |
| `.agent/runbooks/test-to-workflow.md` | A test suite is the most honest specification a project has. Prose drifts and nobody notices; a test |
| `.agent/runbooks/tool-usage-capture.md` | Most repositories already contain the operational knowledge someone needs. It is scattered across |
| `.agent/runbooks/ui-audit.md` | Every user-visible change, and before declaring release readiness. |
| `.agent/runbooks/untrusted-content.md` | how to treat repository content as data, not instructions (CORE-32) |
| `.agent/runbooks/workflow-discipline.md` | A rulebook that is read and then not followed is worse than no rulebook, because it produces the |
| `.agent/runs.schema.md` | One line per execution, in .agent/runs/<YYYY-MM>.jsonl. |
| `.agent/runs/.gitkeep` | Shipped C2GRAPH repository artifact. |
| `.agent/runtime-artifacts.txt` | declares files a run creates, so "not yet" and "wrong" are different defects |
| `.agent/scopes/_TEMPLATE.md` | <trigger keywords that exist in the AGENTS.md routing table> |
| `.agent/scopes/backend-api.md` | Endpoints, services, authentication, authorization, background jobs, third-party integrations. |
| `.agent/scopes/data-schema.md` | Tables, columns, indexes, relations, migrations, seeds, heavy queries. |
| `.agent/scopes/frontend-web.md` | Pages, components, client routing, client state, styling, assets. |
| `.agent/scopes/infra-deploy.md` | Environments, secrets, DNS, CDN, pipelines, runtime configuration, deploys. |
| `.agent/scopes/mobile-android.md` | Any work touching Android sources, Gradle configuration, Android resources, Play Console metadata, |
| `.agent/scopes/mobile-ios.md` | Any work touching iOS sources, Xcode project configuration, asset catalogs, entitlements, |
| `.agent/scopes/mobile-native.md` | Mobile work that is not specific to one platform: shared business logic, cross-platform |
| `.agent/scopes/payments-sensitive.md` | The highest-standard scope in this repo. If you are in doubt inside this scope, stop. |
| `.agent/scopes/qa-tooling.md` | Tests, QA harnesses, browser automation, tool integration, CI. |
| `.agent/scopes/ui-ux.md` | Visual decisions, typography, color, spacing, icons, product naming, copy tone. |
| `.agent/state/.gitkeep` | Handoff state lives here. Created by the adoption prompts, written by |
| `.agent/toolmap.schema.md` | .agent/toolmap.jsonl records not merely which tools exist, but how this repo actually uses |
| `.gitattributes` | Normalize line endings so the rulebook hash does not change just because of the platform. |
| `AGENTS.md` | ROOT: {{REPO_ROOT}} - the only authoritative tree. |
| `BOOTSTRAP-NEW-REPO.prompt.md` | Empty or brand-new repo bootstrap; goal inheritance is inventoried before verification and applied only in PHASE 8. |
| `CHANGELOG.md` | All notable changes to this template are recorded here. |
| `CHECKLIST.md` | Climb one level at a time. Jumping to L4 without L2 produces a beautiful graph that cannot prove |
| `CODEOWNERS` | who reviews which part of the corpus |
| `CONTRIBUTING.md` | what a change must pass before it is accepted |
| `INSTALL-WORKFLOW-ONLY.prompt.md` | **Executable workflow-only entry point.** Give this wrapper to the agent when an existing non-empty repo needs only C2GRAPH install/repair/upgrade/verification. It is the only entry that authorizes `ENTRY_MODE=WORKFLOW_ONLY`; it preserves all goal records and forbids product changes. |
| `LICENSE` | MIT |
| `PLACEHOLDERS.md` | Every token below appears in AGENTS.md, the scopes, the runbooks, and the prompts. |
| `README.md` | A template that lets a software project be worked on by agents in a way that is measurable, |
| `SCAN-AND-ADOPT.prompt.md` | Existing repo, goal-emitting adoption path. **Direct execution means `ENTRY_MODE=ADOPT_WITH_GOAL`; do not invoke this file directly for workflow-only.** The workflow-only wrapper may delegate to it internally. |
| `SECURITY.md` | how to report a vulnerability, and what the checksums do and do not prove |
| `START-HERE.md` | Chooses among three entry prompts: goal-emitting adoption, workflow-only installation, or new-repo bootstrap. |
| `adapters/c2modeladapter/MODEL-COVERAGE.md` | Runtime-scoped profile inventory and explicit capability limits. |
| `adapters/c2modeladapter/README.md` | Embedded reference adapter usage and boundary documentation. |
| `adapters/c2modeladapter/c2modeladapter/__init__.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/base.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/cache.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/errors.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/factory.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/open_weight.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/probe.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/providers.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/registry.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/structured.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/types.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/c2modeladapter/validation.py` | Reference adapter implementation module for protocol/runtime/cache normalization. |
| `adapters/c2modeladapter/pyproject.toml` | Reference adapter Python package metadata; engineering revision v0.5.5. |
| `adapters/c2modeladapter/simulation/run_simulation.py` | Deterministic/randomized protocol and cache-plan simulation harness. |
| `adapters/c2modeladapter/simulation/simulation-results.json` | Shipped configuration, schema, ledger example or generated measurement surface. |
| `adapters/c2modeladapter/tests/test_adapter.py` | Reference adapter unit/regression tests. |
| `docs/HOST-COMPATIBILITY.md` | what was actually tested on which host, with dates |
| `docs/graph-engineering/00-audit.md` | Filled in by prompts/00-baseline-audit.prompt.md. Read-only with respect to the repo. |
| `docs/graph-engineering/01-measurable-loop.md` | Filled in by prompts/01-measurable-loop.prompt.md. This is the step that must never be skipped. |
| `docs/graph-engineering/02-lessons.md` | Filled in by prompts/02-lessons-corpus.prompt.md. |
| `docs/graph-engineering/03-graph-plane.md` | Filled in by prompts/03-graph-plane.prompt.md. |
| `docs/graph-engineering/04-context-budget.md` | Filled in by prompts/04-context-split.prompt.md. |
| `docs/graph-engineering/05-retrieval-eval.md` | Filled in by prompts/05-retrieval-eval.prompt.md. |
| `docs/graph-engineering/06-operations.md` | Filled in by prompts/06-operations.prompt.md. |
| `docs/graph-engineering/07-system-inventory.md` | Filled in by PHASE 0 of SCAN-AND-ADOPT.prompt.md or BOOTSTRAP-NEW-REPO.prompt.md. |
| `docs/graph-engineering/08-repo-scan.md` | Filled in by PHASE 1 and PHASE 2 of SCAN-AND-ADOPT.prompt.md. Read-only with respect to the repo. |
| `docs/graph-engineering/09-plan-scan.md` | Filled in by PHASE 1 of BOOTSTRAP-NEW-REPO.prompt.md, for a repo that contains no code yet. |
| `docs/graph-engineering/10-architecture-selection.md` | This document records which architecture is used for which class of work, and why. Its purpose is |
| `docs/graph-engineering/11-complexity-budget.md` | Defines proactive resource boundaries so a run suspends resumably before exhaustion instead of losing continuity. |
| `docs/graph-engineering/12-compaction-handoff.md` | Every agent host has a finite context window. When it fills, the host must choose between failing |
| `docs/graph-engineering/13-goal-contract.md` | A plan and a goal answer different questions, and confusing them is why implementation drifts. |
| `docs/graph-engineering/14-tool-usage-map.md` | Chapter 07 answers what is installed on this machine, and what may therefore be claimed. This |
| `docs/graph-engineering/15-test-workflow-map.md` | A test suite is a specification that cannot drift silently. Prose describing behaviour can be wrong |
| `docs/graph-engineering/16-mobile-platform.md` | Web and mobile differ in one respect that reshapes every procedure built on top of it: |
| `docs/graph-engineering/17-workflow-discipline.md` | A template like this one can be complete, coherent, and entirely ignored. The failure is not that |
| `docs/graph-engineering/18-scan-automation.md` | Which phases a script can do outright, which stay with the agent, why old and new repos need different scanners, and why READ-ONLY never meant do-not-execute |
| `docs/graph-engineering/19-bounded-read.md` | Engineering design and claim boundary. |
| `docs/graph-engineering/20-model-adapter.md` | Engineering rationale and C2GRAPH authority boundary for host model adapters. |
| `docs/graph-engineering/_decisions.md` | Optional durable PHASE 3 decision ledger. Answers may remain ordered `PENDING_DECISION` session evidence until this exact path is intentionally write-planned and authorized; inability to persist it does not stop the question queue. |
| `docs/graph-engineering/_ledger.jsonl` | append-only ledger of graph-engineering outputs |
| `docs/graph-engineering/model-adapter-provider-contracts.md` | Audited provider/runtime cache-contract matrix and freshness boundary. |
| `docs/graph-engineering/references.md` | This document records where the ideas in this package came from, so that a rule can be traced to |
| `docs/guides/.gitkeep` | Verified-behaviour guides land here, one file per lane, named |
| `eval/retrieval/golden.example.jsonl` | {"id":"Q001","question":"In which file is the handler for the checkout route defined?","kind":"exact_route","partition":"api","expected":["src/api/rou... |
| `eval/rubrics/_TEMPLATE.md` | <which artifact or task> |
| `prompts/00-baseline-audit.prompt.md` | Read-only with respect to the repo. The only write that is permitted: |
| `prompts/01-measurable-loop.prompt.md` | Prerequisite: PROMPT 00 is complete. Do not skip this prompt. Every later prompt produces |
| `prompts/02-lessons-corpus.prompt.md` | Prerequisite: PROMPT 00. Its value is proportional to the size of the lesson notes that already |
| `prompts/03-graph-plane.prompt.md` | Prerequisite: PROMPT 01 has been running for at least 6 weeks. Running this too early produces a |
| `prompts/04-context-split.prompt.md` | Prerequisites: PROMPT 00, and PROMPT 01 already has a baseline. This is the riskiest prompt in the |
| `prompts/05-retrieval-eval.prompt.md` | Prerequisites: PROMPT 04, and the corpus partitions are already defined. |
| `prompts/06-operations.prompt.md` | Prerequisites: PROMPT 01 and PROMPT 04. |
| `prompts/07-compaction-drill.prompt.md` | A rehearsal. You run this deliberately, on a real task, to find out whether the handoff mechanism |
| `prompts/08-goal-emit.prompt.md` | Paste this when the requirements are complete and you want an implementation that runs without |
| `prompts/97-budget-resume.prompt.md` | Resume a `suspended_resumable` run from a durable handoff or portable capsule without restarting PHASE/FASE 0. |
| `prompts/98-compaction-handoff.prompt.md` | Two prompts. Paste 98A the moment you see any sign of compaction, or before a long run. |
| `prompts/99-session-resume.prompt.md` | Used when continuing work that has been running for a long time, especially after the rulebook |
| `scripts/README.md` | Splits the two kinds: 29 shipped validators that run as-is, and 23 contracts you implement because language and runtime differ per project |
| `scripts/adapter/capability.py` | Read-only capability-document validator and canonical digest generator. |
| `scripts/budget/check.py` | Read-only per-resource-epoch classifier: `CONTINUE`, `HANDOFF_NOW`, or `HARD_EXHAUSTED`; reports epoch and cumulative audit usage. |
| `scripts/c2python.cmd` | Canonical Windows Python launcher; validates Python >=3.8 and selects configured runtime, `py -3`, `python.exe`, then `python3.exe`. |
| `scripts/c2python.sh` | Canonical POSIX Python launcher; validates Python >=3.8 and selects configured runtime, `python3`, then `python`. |
| `scripts/goal/gate.py` | Validates goal-ledger integrity by default; `--require-active` additionally gates later product implementation. |
| `scripts/graph/validate.py` | graph schema against graph instance, both directions |
| `scripts/handoff/capsule.py` | Read-only validator for `C2GRAPH_RESUME_CAPSULE v1`; rejects percentages and restart/rescan next actions. |
| `scripts/handoff/validate.py` | schema, live checkpoint and canary nonce agree |
| `scripts/install/preflight.py` | two-stage target-byte-read-only rollback classifier: preliminary basis first, then exact `--plan-op`/`--plan-empty` authority for the actual write set; also refuses unsafe path aliasing/hardlinks |
| `scripts/interview/ask.py` | Optional persisted-queue renderer/collector; not a pre-3.W prerequisite; answer writes require rollback authority plus post-plan semantic path authorization. |
| `scripts/jsonl/validate.py` | one JSON object per line, required keys present |
| `scripts/lessons/decay.py` | lists lessons past their reconfirmation interval |
| `scripts/lib/c2common.py` | shared helper: repo root, distribution-manifest-bounded file walk, config parsing, count reporting |
| `scripts/lib/c2read.py` | Shared bounded read primitives. |
| `scripts/provenance/export.py` | exports the ledger as W3C PROV-O JSON-LD; dry run by default, `--sign` writes a SHA-256 manifest |
| `scripts/read/core.py` | Canonical bounded text evidence CLI. |
| `scripts/scan/census.py` | PHASE 1 census for an EXISTING repo: structure, stack, tests, CI, git health, partition budget |
| `scripts/scan/plan.py` | PHASE 1.1 census for a NEW repo: planning artifacts and structural spans only; secret-prone paths defer to `scan/redact.py`; no raw source-line output |
| `scripts/scan/redact.py` | Secret-safe structural reader for `.env*`, `wrangler*.toml`, credential/secret files; emits line plus identifier-class/fingerprint metadata without raw values, raw identifiers, subprocesses, network, or target writes. |
| `scripts/scan/tooling.py` | PHASE 0 static tool inventory for both repo kinds: 20 tools; spawns no candidate tool, reports `on_path_unverified` / `not_on_path`, and leaves execution proof to post-authority V |
| `scripts/selftest/model_adapter.py` | Adversarial C2GRAPH adapter admission selftest. |
| `scripts/selftest/read.py` | Adversarial read selftest. |
| `scripts/selftest/run.py` | Exercises populated validators plus goal-mode, GATE 0 zero-mutation, two-stage write-plan authority, symlink/hardlink/path-shape safety, CORE-33 routing, and product-corpus-isolation regressions. |
| `scripts/state/lock.py` | TTL lease that makes split brain detectable |
| `scripts/state/migrate.py` | converts state between schema versions, dry run unless `--apply` |
| `scripts/verify/adapter.py` | Cross-surface adapter configuration/schema/route verifier. |
| `scripts/verify/all.py` | runs the 26 registered checks, prints every count, fails closed |
| `scripts/verify/ambiguity.py` | Rejects advisory wording in normative rule surfaces. |
| `scripts/verify/claims.py` | compares published prose numbers to the measurement, and tokens to the dictionary |
| `scripts/verify/cli.py` | Checks documented CLI flags against implemented argparse interfaces. |
| `scripts/verify/compliance.py` | the control map against the tree: every rule exists, every control identifier has the shape its framework publishes, every evidence path is present, the caveat is intact |
| `scripts/verify/counts.py` | measures every number the package publishes; `--write` stores the snapshot |
| `scripts/verify/discipline.py` | Checks workflow-discipline contracts, entry prompts, goal classes, blockers, and ordering gates. |
| `scripts/verify/enforcement.py` | Measures how many CORE rules are mechanically named by validators and enforces the ratchet. |
| `scripts/verify/excludes.py` | exclude rules do not contradict the carve-outs that need read access |
| `scripts/verify/golden.py` | Validates the retrieval golden-set shape and coverage floor. |
| `scripts/verify/hygiene.py` | file hygiene: ASCII body, no tab, no trailing whitespace, LF, final newline, no BOM |
| `scripts/verify/index_scope.py` | refuses a root index in a monorepo, an application with no index, and an index directory that is not excluded |
| `scripts/verify/links.py` | every relative Markdown link resolves |
| `scripts/verify/paths.py` | every path literal resolves, or is declared in `.agent/runtime-artifacts.txt` |
| `scripts/verify/placeholders.py` | unresolved placeholders in any form, in both languages |
| `scripts/verify/prefix.py` | Checks the router skeleton against `.agent/prefix.lock`; `--relock` records intentional changes. |
| `scripts/verify/preserve.py` | the baseline against the registry against the router: no shipped rule disappears silently, relocations name a destination that exists, rejections need a human |
| `scripts/verify/provenance.py` | the decision ledger against its schema: field set, digest chain, and a named acceptor on every accepted record |
| `scripts/verify/read.py` | Cross-surface read contract validator. |
| `scripts/verify/recall.py` | Validates failure-memory classes, retry policy, age, and promotion obligations. |
| `scripts/verify/routes.py` | Checks Markdown routing rows against `.agent/routes.yaml` and target existence. |
| `scripts/verify/width.py` | the width fence: router length and prose line length |

## Current verification snapshot

- Files per edition: **201**
- Python scripts: **46**
- Validators/gates: **32**
- Registered checks: **30**
- Routing rows: **43**
- Runbooks: **36** including template
- Graph-engineering docs: **24**
- CORE rules: **CORE-01..CORE-38** contiguous

The authoritative live count remains `<C2PY> scripts/verify/counts.py --json`.
