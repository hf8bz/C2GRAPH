# C2GRAPH Agent, Discovery and Orchestration Guide

**Bahasa Indonesia:** [C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE-ID.md](C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE-ID.md)

> C2GRAPH v1.2.0 cross-cutting guide. This document covers C2GRAPH-native discovery, indexing, bounded delegation, batching, and reduction. It does not import C2Cognitive-only worker/session features.

## 1. Discovery goal

The purpose of discovery is to identify the smallest evidence-backed repository surface needed for the current objective without mixing unrelated corpora or turning retrieval into authority.

## 2. Discovery ladder

A C2GRAPH-compatible discovery sequence is:

1. bind repository root and entry mode;
2. inventory host/project tooling without silently executing unsafe candidates;
3. classify repo shape and corpus partitions;
4. locate per-application indexes correctly;
5. use bounded scanners/search/graph discovery;
6. use Bounded Evidence Read for large/minified/log/text evidence;
7. use secret-aware redacted structural readers for secret-prone paths;
8. record evidence/provenance and contradictions;
9. reduce findings into the goal/requirement/plan plane;
10. only then consider mutation authority.

## 3. Corpus isolation

Evidence from one application/partition must not silently answer a question about another. Per-application index placement and corpus partitions reduce false joins in monorepos and mixed repositories.

## 4. Discovery cache and failure recall

Previously discovered facts and failure memory can reduce repeated work, but cached/remembered material is not automatically current. Recall must respect age, source evidence, containment, and contradiction/freshness rules. CEA may suppress actionability of poisoned exact recalled failure/lesson objects.

## 5. Graph discovery

C2GRAPH's graph plane represents relationships among objectives, tasks, plans, evidence, requirements, artifacts, claims, decisions, failures, lessons, checkpoints, and other work entities. Graph traversal should be bounded and evidence-linked. A graph relationship is not semantic truth merely because it exists.

## 6. Bounded agent dispatch

Parallel work is useful only when:

- the work can be partitioned;
- success criteria are explicit;
- evidence ownership is clear;
- write ownership does not create uncontrolled concurrent writers;
- a deterministic reduction step exists;
- the latency/cost tradeoff is justified.

Worker output is advisory evidence until the coordinating workflow validates it. Fan-out does not imply fan-out write authority.

## 7. Batching

Batching reduces overhead for repeated independent operations. Batch size and triggers belong to central configuration. A batch must not hide per-item failure, per-path authority, or verification obligations.

## 8. Liveness/self-audit

Visible activity is not semantic progress. The progress-liveness watchdog can launch bounded read-only diagnostic lanes. Those diagnostic workers are advisory and `canonical_write=false`; diagnosis cannot self-authorize repair.

## 9. Reduction rule

All multi-source/multi-agent discovery must converge through an explicit reduction step that preserves:

- source identity;
- contradictions;
- confidence/claim limits;
- unresolved questions;
- exact next evidence needs;
- and the distinction between knowledge and authority.
