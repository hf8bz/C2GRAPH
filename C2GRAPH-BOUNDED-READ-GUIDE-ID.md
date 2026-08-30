# C2GRAPH v1.2.0 — Panduan Bounded Evidence Read v2

**English:** [C2GRAPH-BOUNDED-READ-GUIDE.md](C2GRAPH-BOUNDED-READ-GUIDE.md)

> Companion Indonesia untuk `C2GRAPH-BOUNDED-READ-GUIDE.md`. Bounded Evidence Read v2 mengatur bagaimana byte repository menjadi evidence agent tanpa mengganti Goal, rollback, secret, pre-write, atau effect-time authority.

## Command canonical

```text
<C2PY> scripts/read/core.py --path <PATH>
```

Optional controls mencakup `--target`, `--line`, `--char`, `--max-lines`, `--max-bytes`, `--max-chars-per-line`, dan `--json`.

## Physical evidence contract

Default view mempunyai tiga ceiling independen:

- 2,000 physical lines;
- 131,072 normalized UTF-8 output bytes;
- 2,000 characters per physical line.

Human coordinate tetap `(line, char)`, sedangkan v2 continuation juga mengikat source-byte offset, source-generation digest, ceiling-profile digest, monotonic sequence, dan prior ephemeral view digest.

## Typed outcomes

Reader membedakan outcome seperti `CONTENT`, `EMPTY`, `PAST_EOF`, `FORMAT_REDIRECT`, `NOT_FOUND`, `AMBIGUOUS_PATH`, `REPAIR_SEARCH_BOUNDED`, `REFUSED_SPECIAL`, `PATH_ESCAPE`, `NOT_REGULAR_FILE`, `CHANGED_DURING_READ`, `IO_FAILURE`, `STALE_CURSOR`, `PROFILE_MISMATCH`, `STRATEGY_REVIEW_REQUIRED`, serta CLI `SECRET_PRONE_REFUSED`.

Missing file tidak boleh disamakan dengan empty file.

## Filesystem/format safety

Containment diverifikasi sebelum content I/O. Special/device/escape surface ditolak. PDF/image/archive/executable signatures, NUL-bearing input, dan invalid UTF-8 diarahkan sebagai format yang tidak boleh diperlakukan sebagai raw text evidence.

Control ini mengurangi race/escape surface tetapi bukan bukti semua hostile filesystem race mustahil.

## Freshness

View diikat ke fingerprint file. Perubahan saat read menghasilkan `CHANGED_DURING_READ`; generation mismatch pada continuation menghasilkan `STALE_CURSOR`; perubahan ceiling profile menghasilkan `PROFILE_MISMATCH`.

Tidak ada persistent ledger yang mengklaim “model sudah melihat file ini”. Read receipt bersifat ephemeral; read tidak otomatis memutasi canonical state.

## Scanner integration

`scan/census.py` dan `scan/plan.py` menggunakan bounded textual inspection. Bila coverage bounded, ketidakmunculan keyword tidak boleh dinaikkan menjadi bukti bahwa keyword tidak ada di seluruh artifact.

## Performance interpretation

Public EN guide menerbitkan benchmark lokal yang menunjukkan bounded read dapat sangat mengurangi first-view bytes pada file besar/minified/wide, tetapi pada file kecil terdapat negative latency result karena containment/fingerprinting/typed receipt overhead. Karena itu Bounded Read v2 bukan universal speed optimization; nilai utamanya adalah bounded evidence contract.
