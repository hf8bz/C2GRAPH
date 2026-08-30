# Referensi C2GRAPH v1.2.0 — Bahasa Indonesia

**English:** [C2GRAPH-REFERENCE.md](C2GRAPH-REFERENCE.md)

> Companion navigasional/semantik untuk `C2GRAPH-REFERENCE.md`. Identifier, CORE rule, gate name, command, schema field, dan numeric threshold tetap menggunakan token canonical English agar tidak terjadi translation drift. Bila angka/identifier perlu dipakai secara executable, periksa canonical source owner.

## 1. Authority model

C2GRAPH memisahkan objective, evidence, scope, plan, exact write authority, rollback, freshness, runtime strategy, dan verification. Knowledge/strategy state tidak otomatis memperlebar authority.

## 2. CORE rules

Public Reference menginventarisasi CORE rule baseline `CORE-01` sampai `CORE-39` secara contiguous. Rule identifier dipertahankan lintas bahasa; companion ID menjelaskan semantics tetapi tidak mengganti normative source.

## 3. Entry modes

Formal mode:

- `ADOPT_WITH_GOAL`;
- `WORKFLOW_ONLY`;
- `BOOTSTRAP`.

`SCAN-AND-ADOPT.prompt.md` adalah prompt name untuk direct adoption path, bukan mode bernama `SCAN`.

## 4. Mutation gates

Normal write predicate tetap bergantung pada semantic scope, exact `ACTUAL_WRITE_SET`, rollback basis, fresh pre-state/effect-time authority, dan verification. GATE 0 bersifat mutation-free.

## 5. Central configuration

`.agent/config.yml` adalah owner tunable project/runtime thresholds. Gunakan `C2GRAPH-CONFIGURATION-REFERENCE-ID.md` untuk keluarga konfigurasi dan canonical Reference untuk exact current values.

## 6. Bounded Evidence Read v2

Default physical view contract yang dipublikasikan: 2,000 lines, 131,072 normalized UTF-8 output bytes, dan 2,000 characters/physical line, dengan typed outcomes, generation-bound continuation, containment, format redirect, stale cursor detection, dan no persistent “model has seen file” belief.

## 7. Graph vocabulary

Public Reference menyatakan 20 node types dan 20 edge types. Graph relationships harus valid terhadap schema dan tidak otomatis menjadi semantic truth.

## 8. Provenance

Provenance surface mendukung run/source/model/provider/phase/decision/human acceptance/digest lineage dan PROV-O export. Digest chaining adalah integrity evidence, bukan identity proof.

## 9. Model adapter, BEA, ACRP, CEA, liveness

- Model adapter mengelola runtime capability/protocol/cache boundary.
- BEA memberi bounded human-granted emergency overlay terhadap exact plan/action/target.
- ACRP memilih representation strategy tanpa mengubah semantic evidence set/order.
- CEA membatasi actionability recalled failure/lesson object.
- Progress Liveness/self-audit menilai progress state tanpa menciptakan write row.

## 10. Strict runtime numeric validation

State yang mensyaratkan integer/number harus menolak numeric string, boolean-as-number, invalid fractional integer, dan non-finite numeric state sesuai schema/validator contract.

## 11. Handoff/event integrity

Current checkpoint row tidak dimutasi untuk resume telemetry. Resume event terpisah menjaga ledger integrity. Exact measurement yang tidak tersedia dicatat unavailable, bukan diperkirakan.

## 12. Verification & assurance snapshot

Gunakan engineering wording “verification and assurance”. Pertahankan measured results serta caveat finite-domain/harness boundaries dari canonical Reference dan v1.2.0 Release Notes, lalu arahkan pembaca ke surface verification/assurance yang baru.

## 13. Catalog terkait

- `C2GRAPH-CONTROL-PLANE-CATALOG-ID.md`
- `C2GRAPH-SCHEMA-STATE-CATALOG-ID.md`
- `C2GRAPH-RUNBOOK-CATALOG-ID.md`
- `C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md`
- `C2GRAPH-REPOSITORY-COVERAGE-MATRIX-ID.md`
