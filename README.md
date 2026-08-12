# C2GRAPH v1.0.0 Documentation

**Release:** `v1.0.0` - Version 1, released **12 August 2026**.
**Author:** Hafizh Al-Banna - [LinkedIn](http://www.linkedin.com/in/hafizh-al-banna-63275410)

This README is the release landing page for the C2GRAPH documentation bundle. It describes the current Version 1
surface only. Operational authority remains in the executable prompts, repository rules, configuration, and
verification scripts shipped with the matching language edition.

## Research Paper

C2GRAPH is accompanied by a bilingual academic preprint describing its
motivation, architecture, governance model, execution mechanisms,
experimental methodology, validation results, and limitations.

**Paper:** *C2GRAPH: A Repository-Level Governance Package for Disciplined, Measurable AI Coding Agents*

**DOI:** [10.5281/zenodo.21905651](https://doi.org/10.5281/zenodo.21905651)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21905651.svg)](https://doi.org/10.5281/zenodo.21905651)

Available in:
- English
- Bahasa Indonesia

## What C2GRAPH provides

C2GRAPH is a repository-level governance framework for AI coding agents. It externalizes operating constraints that
otherwise remain implicit in a conversation: goal state, evidence, write authority, rollback scope, context budget,
handoff, provenance, tool discipline, failure memory, and verification.

The Version 1 release includes:

- `ADOPT_WITH_GOAL` for an existing repository that must end with an admitted implementation goal;
- `WORKFLOW_ONLY` for control-plane installation, repair, or verification without product mutation;
- `BOOTSTRAP` for a new or empty repository;
- Bounded Evidence Read v2 for physically bounded text ingestion and generation-bound continuation;
- secret-aware discovery and PHASE 3 safety handling;
- exact `ACTUAL_WRITE_SET` and path-scoped rollback authority;
- blocker convergence and resumable resource epochs;
- evidence/provenance contracts and a typed graph plane;
- a runtime-scoped/cache-aware host model-adapter boundary;
- English and Bahasa Indonesia source editions with synchronized product contracts.

## Documentation map

| Need | Document |
| --- | --- |
| Choose the correct entry mode | `C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md` |
| Install only the C2GRAPH control plane | `C2GRAPH-WORKFLOW-ONLY-GUIDE.md` |
| Give an agent a workflow-only instruction | `C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md` |
| Resolve Python launcher spelling | `C2GRAPH-CLI-LAUNCHER-GUIDE.md` |
| Handle budget/session exhaustion | `C2GRAPH-BUDGET-RESUME-GUIDE.md` |
| Review exact writes and rollback authority | `C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md` |
| Converge blockers | `C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md` |
| Handle secret exposure | `C2GRAPH-SECRET-SAFETY-PHASE3-GUIDE.md` |
| Read large/minified/log evidence safely | `C2GRAPH-BOUNDED-READ-GUIDE.md` |
| Understand the model-adapter boundary | `C2GRAPH-MODEL-ADAPTER-GUIDE.md` |
| Find rules, gates, and thresholds | `C2GRAPH-REFERENCE.md` |
| Understand architecture and rationale | `C2GRAPH-DEEP-DIVE.md` |
| Find shipped files | `C2GRAPH-FILE-MAP.md` |
| Diagnose a stopped run | `C2GRAPH-TROUBLESHOOTING.md` |
| Navigate the bundle | `C2GRAPH-DOCS-START-HERE.md` |

## Measured release shape

Each language edition contains **198 files**, **46 shipped Python scripts**, **32 validators/gates**, **30 registered
checks**, **43 routes**, **36 runbooks including the template**, **24 graph-engineering documents**, **223
configuration keys**, and `CORE-01..CORE-38`.

The documentation bundle contains **17 files**, including this README and the model-adapter guide. The flowchart
bundle contains **66 flowcharts plus an index**, with 66 DOT, 66 PNG, and 66 SVG render targets.

## Model-adapter boundary

Provider-native parsing, streaming assembly, native tool-call identifiers, private reasoning state, and provider
cache controls remain host-side. C2GRAPH accepts bounded provider-neutral capability evidence. Cache telemetry is
performance evidence only and never authorizes goal changes, writes, rollback, completion, or freshness.

## Verification boundary

Completed release runs reported **30/30 registered checks and 0 failures** per language edition, with four clean-
template observations explicitly classified as vacuous. Bounded-read selftest completed **56/56** and model-adapter
admission selftest **23/23** per edition.

These results are bounded to the executed deterministic/process surfaces. They do not prove universal correctness or
semantic model quality.
