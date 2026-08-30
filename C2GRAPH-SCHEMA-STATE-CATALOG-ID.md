# Katalog Schema dan Runtime State C2GRAPH

**English:** [C2GRAPH-SCHEMA-STATE-CATALOG.md](C2GRAPH-SCHEMA-STATE-CATALOG.md)

> Peta public state C2GRAPH v1.2.0. Dokumen membedakan shipped schema/template, durable ledger, generated runtime path, dan non-authoritative state.

## 1. Aturan inti

State dapat mempengaruhi pekerjaan berikutnya tanpa otomatis menjadi write authority. Checkpoint, cache hit, failure memory, lesson, graph edge, atau provenance record adalah evidence/state; mutation tetap memerlukan semantic scope + exact write plan + rollback + freshness + verification.

## 2. Goal state

- `.agent/goals.schema.md`
- `.agent/goals/`

Goal menyimpan objective/lineage. Ia mempersempit intent, bukan permission untuk arbitrary writes.

## 3. Requirement state

- `.agent/requirements.schema.md`

Requirement harus mempertahankan provenance dan membedakan received/derived/inferred state.

## 4. Work graph

- `.agent/graph/schema/nodes.md`
- `.agent/graph/schema/edges.md`
- `.agent/graph/schema/example.jsonl`
- `.agent/graph/`

Public Reference menyebut 20 node types dan 20 edge types. Schema validator harus menolak undeclared type.

## 5. Failure dan lesson

- `.agent/failures.jsonl`
- `.agent/failures.schema.md`
- `.agent/lessons.jsonl`
- `.agent/lessons.schema.md`

Failure memory dan lesson mempunyai fungsi berbeda dari current evidence. Recall perlu freshness/reconfirmation rules.

## 6. Handoff/resume

Handoff/checkpoint schema menjaga continuity. Current checkpoint row tidak ditulis ulang untuk memasukkan resume telemetry; event baru digunakan bila persistence sah. Missing exact measurement harus direkam unavailable, bukan diestimasi.

## 7. Provenance dan adapter/tool state

Provenance schema/ledger, model-adapter capability state, routes, tool map, run state, config, dan generated counts mempunyai authority class berbeda. Dokumentasi harus mempertahankan perbedaan antara observed/generated state dengan normative authority.
