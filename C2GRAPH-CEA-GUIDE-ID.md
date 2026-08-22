# C2GRAPH Cognitive Emergency Authorization (CEA)

**Status rilis:** termasuk dalam C2GRAPH v1.2.0. CEA C2GRAPH sengaja lebih sempit daripada CEA C2Cognitive.

## Mengapa C2GRAPH membutuhkan CEA yang sempit

C2GRAPH mempunyai ledger failure-memory dan lesson durable yang dapat mempengaruhi recall berikutnya. Saat insiden,
exact
record `FAIL-*` atau `LSN-*` yang teracun mungkin harus segera berhenti menjadi actionable. Ledger tersebut adalah byte
repository, sehingga memberi CEA hak rewrite langsung akan membuat repository-write authority kedua di samping BEA.

Karena itu C2GRAPH memakai overlay containment eksternal:

```text
incident evidence
  -> exact failure/lesson ID + canonical record digest
  -> approval CEA manusia eksplisit
  -> grant singkat dan non-transferable
  -> external suppression overlay
  -> selector recall menjaga raw evidence tetapi actionable=false
```

## Pemisahan authority

CEA tidak mengotorisasi repository/product write, tidak mewarisi BEA, tidak meningkatkan trust, tidak memperlebar ACL,
tidak override freshness, dan tidak mendapat authority dari ACRP/cache/model routing. Koreksi/penghapusan durable pada
ledger tetap membutuhkan normal write authority atau BEA. Jika konten teracun sudah dikonsumsi model, host reset/sesi
baru wajib dilakukan sebelum containment dianggap efektif.

Wildcard dan suppression per-class/signature dilarang. Setiap object terikat digest record saat ini; mutasi record
menginvalidasi grant. Grant terikat run/session/incident, kedaluwarsa maksimal 900 detik, dan tidak berlaku setelah
incident resolved/revoked.

## Verifikasi

```text
<C2PY> scripts/verify/cognitive_emergency.py
<C2PY> scripts/selftest/cognitive_emergency.py
<C2PY> scripts/selftest/fullstack_interactions.py
```

Kandidat saat ini memverifikasi raw-evidence retention, exact-ID/digest binding, penolakan cross-session dan incident
resolved, wildcard refusal, external-only output, consumed-content reset, serta pemisahan CEA/BEA/ACRP/cache/model.
## Hardening CLI 19 Agustus

Audit function-surface menemukan dan menutup `C2G-CEA-20260819-02`: `main()` menggunakan kembali alias import
`c2common` untuk subparser `check` dan dapat gagal sebelum command handling. Variabel parser lokal diganti nama, dan
selftest CEA kini benar-benar mengeksekusi jalur entry CLI sebagai regression case. Evidence directed saat ini adalah
**18/18 per edisi**. Ini merupakan defect availability/correctness; tidak ditemukan jalur pelebaran authority karena
eksekusi gagal sebelum authorization.

