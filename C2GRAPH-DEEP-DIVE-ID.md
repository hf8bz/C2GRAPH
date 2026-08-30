# C2GRAPH Deep Dive — Bahasa Indonesia

**English:** [C2GRAPH-DEEP-DIVE.md](C2GRAPH-DEEP-DIVE.md)

> Companion arsitektur publik C2GRAPH v1.2.0. Dokumen ini menjelaskan hubungan subsistem; executable source contract tetap authoritative untuk operasi.

## 1. Masalah yang diselesaikan

Long-running coding agent tidak hanya menghadapi masalah kemampuan model. Ia juga menghadapi objective drift, evidence mixing, repeated discovery, stale state, failure retry, authority widening, rollback loss, dan completion claim yang tidak didukung evidence.

C2GRAPH mengubah masalah tersebut menjadi explicit repository contracts dan state transitions yang dapat diperiksa.

## 2. Tiga lapisan konteks

Secara konseptual C2GRAPH memisahkan:

- durable repository governance/state;
- bounded evidence/working context;
- transient model conversation/runtime representation.

Pemisahan ini memungkinkan compaction/model switch/session boundary tanpa menganggap conversational memory sebagai sumber kebenaran tunggal.

## 3. Knowledge bukan authority

Search result, graph edge, Goal, cache hit, failure memory, checkpoint, ACRP decision, CEA state, liveness classification, atau successful tool call dapat menjadi evidence/strategy input. Tidak satupun otomatis menjadi permission untuk arbitrary repository mutation.

## 4. Entry modes

Tiga formal mode adalah `ADOPT_WITH_GOAL`, `WORKFLOW_ONLY`, dan `BOOTSTRAP`. Mereka menentukan lifecycle awal, bukan menghapus normal mutation contract.

## 5. Goal → evidence → requirement → plan → mutation

C2GRAPH memisahkan objective dari evidence, requirement provenance, plan, exact write authority, rollback, effect-time pre-state, dan verification. Ini mencegah inference berubah menjadi requirement atau plan berubah menjadi wildcard write set.

## 6. Typed Graph Plane

Graph Plane menghubungkan objective, task, run, artifact, evidence, claim, decision, failure, lesson, checkpoint, dan resume. Graph database tidak diwajibkan; yang penting relationship dapat diinspeksi dan divalidasi.

## 7. Bounded discovery/read

Corpus partition, per-application index, scanner limits, Bounded Evidence Read v2, secret-aware structural reads, discovery cache, dan completeness backfill mengontrol bagaimana evidence ditemukan tanpa membaca semuanya secara tidak terbatas.

## 8. Failure/lesson/ratchet

Failure Memory menghindari retry buta. Durable Lessons membawa pembelajaran lintas session. Ratchet loop hanya mempertahankan perubahan ketika metric yang didefinisikan benar-benar membaik dan rollback tetap tersedia.

## 9. Model/runtime adaptation

Model adapter dan ACRP dapat menyesuaikan protocol/representation, tetapi semantic evidence set dan authority tetap terpisah. Cache telemetry bukan correctness proof.

## 10. Emergency dan containment

BEA adalah bounded human-granted emergency overlay. CEA membatasi actionability recalled failure/lesson object. Keduanya tidak membuat wildcard write authority.

## 11. Liveness dan resume

Progress Liveness membedakan meaningful progress, wait, long-running work, accounting inconsistency, dan stagnation. Handoff/checkpoint/event ledger menjaga continuity tanpa menulis ulang history.

## 12. Verification boundary

Release verification harus dibaca berdasarkan contract/domain yang diuji. Deterministic, finite-domain, randomized, fuzz, package-integrity, dan live/external evidence mempunyai makna berbeda. “Exhaustive” hanya berlaku pada declared finite domain dan bukan universal bug-absence claim.

Untuk detail, gunakan `C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md` dan `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE-ID.md`.
