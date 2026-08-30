# C2GRAPH Evidence, Provenance and Assurance Guide

**Bahasa Indonesia:** [C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE-ID.md](C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE-ID.md)

> C2GRAPH v1.2.0 public assurance guide. This document explains how evidence supports requirements, decisions, compliance mappings, and completion claims.

## 1. Evidence-first rule

C2GRAPH separates confidence from verification. A finding should say what evidence was observed, where it came from, what it supports, what contradicts it, and what remains outside the evidence boundary.

## 2. Evidence classes

Common evidence surfaces include:

- repository source and configuration;
- tests and test behavior;
- scanner/read output;
- graph relationships with source links;
- Goal/requirement records;
- run/verification output;
- tool capability evidence;
- release/package hash/path checks;
- human decisions;
- external provider/production evidence when explicitly collected.

Do not substitute one class for another. Synthetic simulation is not production behavior; cache telemetry is not correctness; a graph edge is not semantic truth.

## 3. Requirements provenance

A requirement should be linked to received evidence or explicitly marked as derived/proposed. This prevents an agent inference from silently gaining the status of a product requirement.

## 4. Decision provenance

The public Reference documents a 14-field provenance record and SHA-256 chaining. The chain can detect record alteration under the documented digest contract. It does not prove human identity or provide a cryptographic signer identity when key-based signatures are not required.

## 5. W3C PROV-O export

`provenance/export.py` can export the decision ledger as W3C PROV-O JSON-LD. Interoperable representation improves traceability but does not upgrade weak source evidence into stronger evidence.

## 6. Claims and verification

A completion/assurance claim should bind:

- claim text;
- relevant requirement/objective;
- evidence path/output;
- verification command/harness;
- result;
- limitations;
- freshness/time when relevant.

## 7. Compliance evidence mapping

C2GRAPH maps evidence/control relationships for frameworks including SOC 2, PCI DSS 4.0, and EU AI Act structures. This is an evidence map, not certification and not legal advice. Compliance depends on the broader organization/system and applicable obligations.

## 8. Human decision boundary

Automation should stop or narrow when semantics cannot be established mechanically. A human answer becomes usable governance evidence only within the workflow contract; it does not waive unrelated scope/rollback/freshness requirements.

## 9. Verification and release assurance

Interpret verification evidence by evidence class rather than by a single blanket word such as “tested”. Deterministic checks, declared finite-domain enumeration, randomized exercises, fuzz/adversarial harnesses, package integrity checks, and external/live observations answer different questions.

Use `C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md` for executable verification surfaces and the v1.2.0 Release Notes for the frozen release snapshot. “Exhaustive” is valid only when the declared finite domain is named; it is not a claim about every possible real-world state.

## 10. Assurance anti-patterns

Reject these transformations:

- PASS -> universal correctness;
- finite-domain exhaustive -> all real-world cases;
- source count -> performance improvement;
- protocol conformance -> correct product decision;
- compliance mapping -> certification;
- rollback capability -> permission to write;
- provider cache hit -> valid reasoning/evidence.
