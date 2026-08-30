# Panduan Model Adapter C2GRAPH

**English:** [C2GRAPH-MODEL-ADAPTER-GUIDE.md](C2GRAPH-MODEL-ADAPTER-GUIDE.md)

> Companion Bahasa Indonesia untuk runtime-scoped, cache-aware model adapter C2GRAPH v1.2.0.

## Tujuan

Adapter menormalisasi capability/runtime differences yang relevan bagi workflow C2GRAPH tanpa membuat provider/model-specific behavior menjadi semantic authority.

## Capability admission

Capability document/state harus divalidasi sebelum digunakan untuk routing. Unknown/invalid capability harus fail closed sesuai contract, bukan diasumsikan kompatibel.

## Runtime scope

Model/provider/tool protocol diperlakukan sebagai runtime concern. Switching/routing/fallback tidak boleh mengganti Goal, evidence membership, exact write set, rollback, atau completion criteria.

## Cache awareness

Cache telemetry dapat mempengaruhi representation/routing efficiency tetapi:

```text
cache hit != correctness evidence
cache hit != write authority
cache telemetry != proof of lower cost
```

ACRP juga menjaga semantic evidence set/order ketika representation strategy berubah.

## Stable adapter boundary

Gunakan adapter version/capability yang dinyatakan oleh current C2GRAPH source contract. Dokumentasi tidak boleh mengiklankan adapter version yang tidak ada di release.

## Verification

Selftest/verify adapter harus memeriksa capability/schema/routing invariants. PASS membuktikan tested adapter contract, bukan semantic tool-choice quality atau live-provider availability universal.
