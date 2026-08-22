# C2GRAPH Adaptive Context Representation Planning (ACRP)

**Status rilis:** termasuk dalam C2GRAPH v1.2.0. Capability ini bukan bagian dari byte release historis v1.1.0.

## Tujuan

ACRP mengoptimalkan cara semantic set yang sudah dibatasi dan dibekukan C2GRAPH disajikan ke model efektif. ACRP tidak
menentukan evidence apa yang masuk, tidak memperluas authority, dan tidak mengubah telemetry cache/model menjadi bukti
correctness.

Urutan canonical:

```text
bounded retrieval + seleksi trust/freshness/evidence
  -> freeze ordered semantic set dan selection digest
  -> ACRP
  -> effective-route model adapter
  -> LLM
```

## Mode runtime

- `native`: tanpa transformasi representasi; selalu merupakan jalur aman.
- `canonical`: representasi canonical deterministik tanpa perluasan semantik.
- `segmented`: maksimal satu codec yang sudah admitted untuk setiap segmen eligible.
- `aggressive`: dinonaktifkan pada v1.

Authority, evidence, rollback, source code, security policy, artefak BEA, dan artefak suppression CEA tetap native.
Profile segmented wajib fresh, cocok dengan route, terikat stable adapter representation contract, dan didukung evidence
live-model eksternal untuk semantic/productivity. Evidence yang hilang/basi/tidak cocok kembali ke native/canonical.

## Failover

Alias fisik seperti suffix Luna yang bounded dapat memakai ulang semantic representation profile ketika kontrak model
canonical/runtime yang stabil tidak berubah, sementara identitas cache provider fisik tetap terisolasi. Failover lintas
family menginvalidasi profile lama. Jika family efektif baru tidak punya profile admitted, ACRP memakai jalur native.

## Batas keselamatan

ACRP hanya menerima `c2graph_context_selection_v1` dengan `selection_frozen=true`. ACRP tidak dapat menambah record
setelah compression, mengubah order record, memperluas ACL/trust/freshness, mengotorisasi write, atau memenuhi completion.
File payload/plan hanya boleh dimaterialisasi ke path absolut baru di luar repository.

## Verifikasi

```text
<C2PY> scripts/verify/context_representation.py
<C2PY> scripts/selftest/context_representation.py
<C2PY> scripts/selftest/fullstack_interactions.py
```

Selftest kandidat saat ini mencakup exact round-trip, mismatch route/profile, failover lintas family, tamper/truncation,
preservasi missing-vs-null, keamanan output path, dan 1.000 kasus acak exact/order/determinism.
