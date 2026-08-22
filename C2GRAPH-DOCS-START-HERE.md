# C2GRAPH Documentation — Start Here

> **Identity:** C2GRAPH `v1.2.0` is the current Version 1 minor release dated **22 August 2026**. It adds
> BEA v2, ACRP, C2GRAPH-native CEA, Progress Liveness & Bounded Self-Audit, strict numeric-boundary hardening,
> and the repaired CEA CLI entry path. The v1.1.0 release remains immutable historical provenance.

> **CLI notation:** `<C2PY>` is documentation notation. Resolve it for the current host/session with
> `C2GRAPH-CLI-LAUNCHER-GUIDE.md`; do not type `<C2PY>` literally.

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
| Understand ACRP | `C2GRAPH-ACRP-GUIDE.md` / `C2GRAPH-ACRP-GUIDE-ID.md` |
| Contain poisoned failure/lesson recall | `C2GRAPH-CEA-GUIDE.md` / `C2GRAPH-CEA-GUIDE-ID.md` |
| Diagnose semantic stagnation / bounded self-audit | `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-EN.md` / `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-ID.md` |
| Use bounded emergency write authority | `C2GRAPH-EMERGENCY-AUTHORITY-GUIDE.md` / `C2GRAPH-EMERGENCY-AUTHORITY-GUIDE-ID.md` |
| Find an exact rule, gate or threshold | `C2GRAPH-REFERENCE.md` |
| Understand architecture and design rationale | `C2GRAPH-DEEP-DIVE.md` |
| Find a shipped file | `C2GRAPH-FILE-MAP.md` |
| Diagnose a stopped run | `C2GRAPH-TROUBLESHOOTING.md` |
| Review finite/exhaustive academic validation | `C2GRAPH-ACADEMIC-VALIDATION-GUIDE.md` |
| Read release overview and limitations | `README.md` |

## Model adapter boundary

The v1.2.0 tree retains the host-side runtime-scoped/cache-aware adapter introduced in v1.1.0 without promoting provider
state into C2GRAPH authority. Cache hits and cached-token telemetry are performance evidence only. Raw provider cache
keys, prompt prefixes, and private reasoning are not durable C2GRAPH authority. See
`C2GRAPH-MODEL-ADAPTER-GUIDE.md`.

## Current measured source shape

Per language edition in the C2GRAPH v1.2.0 release: **228 distributed files**, **64 Python scripts**,
**37 validators/gates**, **39 registered checks**, **45 routing rows**, **40 runbooks including the template**,
**27 graph-engineering documents**, and contiguous **CORE-01..CORE-39**. EN router is 205 lines; ID router is 204
lines, both under the 210-line release ceiling.

The external docs bundle contains **26 Markdown files**. Repository-template counts are measured independently from
this documentation bundle.

## Entry-mode short rule

- existing non-empty repository + control-plane maintenance only → `INSTALL-WORKFLOW-ONLY.prompt.md`;
- existing non-empty repository + adoption ending in a new implementation goal → `SCAN-AND-ADOPT.prompt.md`;
- empty/new repository → `BOOTSTRAP-NEW-REPO.prompt.md`.

Goal count alone does not select the entry mode.

## Authority order

Executable prompts and repository rules remain authoritative for execution. Operator docs explain them; they do not
silently override them. When prose numbers disagree with `<C2PY> scripts/verify/counts.py --json`, treat the prose
as stale and investigate the drift.

## C2GRAPH v1.2.0 emergency authority

C2GRAPH v1.2.0 adds Bounded Emergency Authorization (BEA). BEA binds a host-mediated human approval
assertion to the existing canonical PHASE 3.W write plan, rollback evidence, target, run/session, exact actions, and
expiry. It does not create a second `ACTUAL_WRITE_SET`, does not widen semantic scope, and is not a generic break-glass
bypass. Host authentication and authoritative incident-state tracking remain outside the repository mechanism. See
`C2GRAPH-EMERGENCY-AUTHORITY-GUIDE.md` and the Indonesian companion guide.

## C2GRAPH v1.2.0 ACRP and CEA

- `C2GRAPH-ACRP-GUIDE.md` / `C2GRAPH-ACRP-GUIDE-ID.md` - adaptive representation after frozen bounded selection.
- `C2GRAPH-CEA-GUIDE.md` / `C2GRAPH-CEA-GUIDE-ID.md` - exact external containment for poisoned failure/lesson recall.
- CEA, ACRP, cache, and effective model routing never expand BEA authority.

- Progress liveness / self-audit: `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-EN.md` /
  `C2GRAPH-PROGRESS-LIVENESS-SELF-AUDIT-ID.md`
