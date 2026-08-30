# Panduan Agent, Discovery, dan Orchestration C2GRAPH

**English:** [C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md](C2GRAPH-AGENT-DISCOVERY-ORCHESTRATION-GUIDE.md)

> Cross-cutting guide C2GRAPH v1.2.0. Scope: discovery, indexing, bounded delegation, batching, reduction, tool inventory/capture, dan hubungan dengan graph evidence. Dokumen ini tidak mengimpor worker/session feature khusus C2Cognitive.

## 1. Tujuan discovery

Discovery mencari **surface repository terkecil yang didukung evidence** untuk objective saat ini. Retrieval tidak menjadi authority dan evidence dari corpus yang berbeda tidak boleh dicampur tanpa dasar.

## 2. Discovery ladder

Urutan yang kompatibel dengan C2GRAPH:

1. bind repository root dan formal entry mode;
2. inventory tooling/project shape secara bounded tanpa mengeksekusi candidate berisiko secara diam-diam;
3. klasifikasikan repo dan corpus partitions;
4. tempatkan per-application index dengan benar;
5. gunakan scanner/search/graph discovery secara bounded;
6. gunakan Bounded Evidence Read untuk file besar, minified, log, atau text evidence;
7. gunakan structural/redacted reader untuk secret-prone surface;
8. catat provenance, contradiction, dan freshness;
9. reduksi evidence ke goal/requirement/plan plane;
10. baru kemudian evaluasi mutation authority.

## 3. Discovery cache dan failure recall

Cache/failure memory dapat mengurangi pengulangan, tetapi cached fact bukan otomatis current truth. Reuse harus mempertimbangkan source evidence, age, invalidation, containment, dan contradiction.

CEA dapat menahan actionability dari recalled failure/lesson object tertentu tanpa menghapus raw repository evidence.

## 4. Graph discovery

Graph Plane menghubungkan objective, task, plan, evidence, requirement, artifact, claim, decision, failure, lesson, checkpoint, dan entitas kerja lain. Graph traversal harus bounded dan evidence-linked. Edge yang ada bukan otomatis semantic truth.

## 5. Single-agent vs multi-agent

Parallelism hanya masuk akal jika:

- success dapat diverifikasi;
- workstream cukup independen;
- write ownership dapat dipisah;
- alternative hypotheses memang perlu dipertahankan;
- reduction step jelas;
- cost/latency/concurrency sesuai budget.

Lebih banyak agent bukan otomatis lebih benar.

## 6. Bounded dispatch dan reduction

Delegation harus membawa scope, expected output, evidence requirement, budget, dan stop condition. Worker output kembali sebagai evidence/proposal; bukan mutation authority baru. Reduction menyelesaikan contradiction dan menyusun evidence sebelum write plan.

## 7. Tool usage dan system inventory

`system-inventory`, static tool discovery, serta `tool-usage-capture` membantu membedakan alat yang **tersedia** dari alat yang **terbukti dipakai** pada call site nyata. Tool availability tidak membuktikan semantic suitability.

## 8. Coverage backfill

Completeness/backfill adalah prosedur terkontrol. Ketika bounded scan tidak bisa membuktikan completeness, status harus menyatakan limit tersebut dan backfill mengikuti runbook, bukan menebak missing surface.
