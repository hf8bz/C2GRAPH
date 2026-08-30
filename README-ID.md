# C2GRAPH

**English:** [README.md](README.md)

### Convert to Graph Engineering

**Tata kelola tingkat repository untuk AI coding agent yang bekerja dalam horizon panjang.**

C2GRAPH membuat pekerjaan software berbasis agent lebih **bounded, measurable, traceable, resumable, reversible, dan auditable** dengan memindahkan execution state penting dari conversation history sementara ke kontrak repository yang eksplisit.

**Versi 1.2.0 · Current release · 22 Agustus 2026**

- Release Notes: https://github.com/hf8bz/C2GRAPH/releases/tag/v1.2.0
- Current research paper DOI: `10.5281/zenodo.22093405`
- Software DOI: `10.5281/zenodo.22058130`
- English README: [README.md](README.md)
- Indeks dokumentasi: [C2GRAPH-PUBLIC-DOCS-INDEX-ID.md](C2GRAPH-PUBLIC-DOCS-INDEX-ID.md)

---

## Mengapa C2GRAPH ada

Coding agent modern dapat menulis code dengan baik, tetapi pekerjaan long-running memunculkan kelas masalah berbeda:

- objective hilang setelah context compaction;
- repository fact ditemukan berulang kali;
- evidence dari beberapa app/corpus tercampur;
- approach gagal di-retry tanpa new evidence;
- inference berubah diam-diam menjadi requirement;
- tool/model/runtime protocol tidak cocok;
- mutation scope melebar karena adjacent change tampak berguna;
- rollback context hilang;
- completion dilaporkan tanpa evidence yang benar-benar membuktikannya.

C2GRAPH memperlakukan banyak masalah ini sebagai **governance dan state-transition problem yang dapat dibuat eksplisit**.

Prinsip pusatnya:

> **Knowledge tidak otomatis menjadi authority.**

Search result bukan permission to write. Goal bukan arbitrary write authority. Cache hit bukan correctness evidence. Valid tool call bukan bukti tool yang dipilih benar. Rollback mechanism tidak memperluas semantic scope. Checkpoint tidak menciptakan permission untuk mutation. Confidence bukan verification.

---

# Apa itu Graph Engineering?

**C2GRAPH berarti Convert to Graph Engineering.**

Graph Engineering bukan sekadar menambahkan graph database. Ia membuat hubungan penting dalam engineering work menjadi explicit state yang dapat diperiksa:

```text
Objective
   |
   v
 Goal -----------> Requirements
   |                    |
   v                    v
 Task ---> Run ----> Artifact
   |       |             |
   |       v             v
   |    Evidence -----> Claim
   |       |             |
   v       v             v
Decision Metric      Evaluation
   |
   v
Mutation ---> Verification
   |\
   | +----> Failure ---> Lesson
   +------> Checkpoint ---> Resume
```

Tujuannya bukan memasukkan semuanya ke graph, tetapi membuat cukup state eksplisit agar sesi/person berikutnya dapat menjawab objective, evidence, ambiguity, authority, rollback, failure, lesson, resume point, dan completion proof.

---

# Prinsip desain

### 1. Bounded autonomy
Agent bekerja dalam semantic scope, read/write budget, route/scope/runbook, dan stop condition yang dapat diperiksa.

### 2. Evidence before authority
Evidence membantu menentukan scope dan decision; evidence sendiri bukan mutation authority.

### 3. Durable state, bounded context
Goal, requirement provenance, graph, failure memory, lesson, checkpoint, dan handoff dapat bertahan lintas session tanpa menganggap seluruh chat history harus dipertahankan.

### 4. Reversible and verifiable mutation
Write memerlukan exact target, rollback basis, fresh pre-state, effect-time authority, dan post-write verification.

---

# Tiga formal entry mode

- `ADOPT_WITH_GOAL` — existing non-empty repo + objective implementasi nyata. Direct prompt: `SCAN-AND-ADOPT.prompt.md`.
- `WORKFLOW_ONLY` — existing non-empty repo + hanya install/repair/upgrade/verify C2GRAPH. Direct prompt: `INSTALL-WORKFLOW-ONLY.prompt.md`.
- `BOOTSTRAP` — empty/new repo. Direct prompt: `BOOTSTRAP-NEW-REPO.prompt.md`.

`SCAN` bukan mode keempat.

---

# Arsitektur authority ringkas

```text
Objective / Goal
      |
      v
Evidence + Requirements + Provenance
      |
      v
Semantic scope / Plan
      |
      v
Exact ACTUAL_WRITE_SET
 + rollback basis
 + fresh pre-state
      |
      v
Mutation
      |
      v
Immediate verification
      |
      v
Evidence / Claim / Checkpoint / Handoff
```

Routing, cache, ACRP, CEA, liveness, incident severity, dan model adapter dapat mengubah strategy/representation/diagnosis tetapi tidak boleh memperluas exact-write predicate.

---

# 41 capability family C2GRAPH v1.2.0

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
35. Bounded Emergency Authorization (BEA v2)
36. Adaptive Context Representation Planning (ACRP)
37. C2GRAPH-native Cognitive Emergency Authorization (CEA)
38. Progress Liveness & Bounded Self-Audit
39. Strict runtime-state numeric validation
40. Cross-plane authority non-interference
41. Handoff event-ledger integrity and authority-gated resume telemetry

Setelah overlay dokumentasi bilingual ini di-merge, seluruh 41 capability family dipetakan ke owner dokumentasi EN + ID di `C2GRAPH-REPOSITORY-COVERAGE-MATRIX-ID.md`.

---

# Bounded Evidence Read v2

Canonical read CLI:

```text
<C2PY> scripts/read/core.py --path <PATH>
```

Default view: 2,000 physical lines, 131,072 normalized UTF-8 output bytes, dan 2,000 characters per physical line. Continuation terikat source generation/profile sehingga stale view tidak dilanjutkan seolah fresh.

C2GRAPH tidak menyimpan persistent belief “model sudah membaca file ini”. Read evidence bukan canonical state mutation.

---

# Graph, failure, lesson, checkpoint

Typed Graph Plane merepresentasikan engineering relationships tanpa mewajibkan graph database. Failure Memory mencegah blind retry. Durable Lessons menyimpan pembelajaran dengan reconfirmation/freshness discipline. Checkpoint/handoff menyimpan continuity; resume telemetry tidak menulis ulang current checkpoint row.

---

# Model adapter dan adaptive representation

Runtime-scoped/cache-aware adapter membantu menormalisasi model/provider/tool capability. ACRP dapat mengubah representation strategy untuk evidence set yang sudah dipilih tanpa mengubah semantic membership/order. Cache telemetry bukan correctness evidence atau guarantee cost saving.

---

# BEA, CEA, dan Progress Liveness

- **BEA v2**: bounded human-granted emergency overlay yang terikat exact plan/action/target/rollback/run/time; bukan wildcard permission.
- **CEA**: containment terhadap actionability recalled failure/lesson object tertentu; raw evidence tetap terlihat dan history tidak ditulis ulang.
- **Progress Liveness & Bounded Self-Audit**: membedakan progress, wait, long-running work, accounting inconsistency, dan stagnation; diagnosis tidak menciptakan write authority.

---

# Documentation

Public docs sekarang menggunakan navigation layer bilingual. Mulai dari:

- [Indeks Dokumentasi Publik](C2GRAPH-PUBLIC-DOCS-INDEX-ID.md)
- [Mulai Dokumentasi](C2GRAPH-DOCS-START-HERE-ID.md)
- [Matriks Entry Mode](C2GRAPH-ENTRY-MODE-DECISION-MATRIX-ID.md)
- [Deep Dive](C2GRAPH-DEEP-DIVE-ID.md)
- [Reference](C2GRAPH-REFERENCE-ID.md)
- [Matriks Coverage 41 Capability](C2GRAPH-REPOSITORY-COVERAGE-MATRIX-ID.md)

Catalog baru mencakup Control Plane, Configuration, Runbooks, Schema/State, Prompts, Scripts/Verification, serta cross-cutting guides untuk Agent/Discovery/Orchestration, Application Lanes, Graph/State Lifecycle, dan Evidence/Provenance/Assurance.

Dokumentasi menjelaskan sistem. Authority operasional tetap berada pada executable source contracts.

---

# Coverage boundary

Target setelah merge adalah **41/41 capability family mempunyai EN + ID public owner**. Ini tidak sama dengan exact per-file `228/228` claim.

Public File Map masih perlu direkonsiliasi karena melaporkan 40 runbooks tetapi visible table hanya menginventarisasi 36 Markdown runbook bernama, serta melaporkan 64 Python scripts tanpa self-contained exact 64-path table. Missing path tidak boleh ditebak; generate dari authoritative release tree/manifest.

---

# Verification dan evidence

C2GRAPH membedakan deterministic checks, finite-domain enumeration, randomized tests, fuzz/adversarial harnesses, package integrity, process conformance, dan external/live observation. PASS membuktikan contract yang benar-benar diuji; bukan universal correctness.

Verification/assurance tetap berada pada dokumentasi engineering, sedangkan research paper didokumentasikan secara terpisah di section Research/CITATION.

---

# Apa yang bukan C2GRAPH

C2GRAPH bukan:

- foundation model baru;
- klaim bahwa model menjadi intrinsically smarter;
- universal RAG engine;
- requirement untuk graph database;
- alasan memberi unrestricted write access;
- guarantee prompt-cache hit;
- proof of lower API cost;
- solver universal untuk wrong tool choice;
- replacement human judgment;
- compliance certification;
- proof bahwa semua possible bug telah dieliminasi.

---

# Research dan citation

Current C2GRAPH v1.2.0 research manuscript adalah **Manuscript Revision 1**, dipublikasikan **25 Agustus 2026**, author **Hafizh Albanna**:

- Paper DOI: `10.5281/zenodo.22093405`
- Software DOI: `10.5281/zenodo.22058130`
- Software release date: `22 Agustus 2026`

Gunakan `CITATION.cff` untuk citation metadata repository.

---

# License

C2GRAPH dirilis dengan **MIT License**. Lihat [LICENSE](LICENSE).

---

# Author

**Hafizh Albanna**

---

## Prinsip akhir

> **AI agent tidak seharusnya dipercaya hanya karena terdengar yakin.**
>
> **Ia hanya layak dipercaya sejauh objective, evidence, authority, state, provenance, verification, dan rollback-nya dapat diperiksa.**

Itulah **Convert to Graph Engineering**.
