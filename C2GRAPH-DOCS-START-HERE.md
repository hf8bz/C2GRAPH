# C2GRAPH Documentation — Start Here

> **Release identity:** C2GRAPH `v1.0.0`, Version 1 / first public release. The model-adapter integration is an internal revision of the same product release, not C2GRAPH v2.

> **CLI notation:** `<C2PY>` is documentation notation. Resolve it for the current host/session with `C2GRAPH-CLI-LAUNCHER-GUIDE.md`; do not type `<C2PY>` literally.

## Choose the smallest document for the task

| Need | Open |
| --- | --- |
| Install control plane only in an existing repository | `C2GRAPH-WORKFLOW-ONLY-GUIDE.md` + `C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md` |
| Choose SCAN / WORKFLOW_ONLY / BOOTSTRAP | `C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md` |
| Resolve Python launcher spelling | `C2GRAPH-CLI-LAUNCHER-GUIDE.md` |
| Recover from budget/session exhaustion | `C2GRAPH-BUDGET-RESUME-GUIDE.md` |
| Review actual write set / rollback authority | `C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md` |
| Converge blockers without turning false blockers into STOPs | `C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md` |
| Handle secret exposure safely | `C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md` |
| Read large/minified/log evidence | `C2GRAPH-BOUNDED-READ-GUIDE.md` |
| Understand the model-protocol/cache boundary | `C2GRAPH-MODEL-ADAPTER-GUIDE.md` |
| Find an exact rule, gate or threshold | `C2GRAPH-REFERENCE.md` |
| Understand architecture and design rationale | `C2GRAPH-DEEP-DIVE.md` |
| Find a shipped file | `C2GRAPH-FILE-MAP.md` |
| Diagnose a stopped run | `C2GRAPH-TROUBLESHOOTING.md` |
| Read release evidence and limitations | `README.md` |

## Model adapter boundary

The v1.0.0 adapter-integrated tree adds a host-side runtime-scoped/cache-aware adapter without promoting provider state into C2GRAPH authority. Cache hits and cached-token telemetry are performance evidence only. Raw provider cache keys, prompt prefixes and private reasoning are not durable C2GRAPH authority. See `C2GRAPH-MODEL-ADAPTER-GUIDE.md`.

## Current measured source shape

Per language edition: **198 files**, **46 Python scripts**, **32 validators/gates**, **30 registered checks**, **43 routing rows**, **36 runbooks including the template**, **24 graph-engineering documents**, and **CORE-01..CORE-38** unchanged as the product rule range. EN router is 195 lines; ID router is 194 lines, both under the 195-line ceiling.

The docs bundle now contains **17 files** including this index and the model-adapter guide. Repository-template counts are measured independently from this documentation bundle.

## Entry-mode short rule

- existing non-empty repository + control-plane maintenance only → `INSTALL-WORKFLOW-ONLY.prompt.md`;
- existing non-empty repository + adoption ending in a new implementation goal → `SCAN-AND-ADOPT.prompt.md`;
- empty/new repository → `BOOTSTRAP-NEW-REPO.prompt.md`.

Goal count alone does not select the entry mode.

## Authority order

Executable prompts and repository rules remain authoritative for execution. Operator docs explain them; they do not silently override them. When prose numbers disagree with `<C2PY> scripts/verify/counts.py --json`, treat the prose as stale and investigate the drift.
