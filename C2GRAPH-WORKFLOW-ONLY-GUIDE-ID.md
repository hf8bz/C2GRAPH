# Panduan Workflow-Only C2GRAPH

**English:** [C2GRAPH-WORKFLOW-ONLY-GUIDE.md](C2GRAPH-WORKFLOW-ONLY-GUIDE.md)

> Mode ini digunakan pada existing non-empty repository ketika pekerjaan hanya menyentuh C2GRAPH governance/control plane, bukan product behavior.

## Contract utama

`ENTRY_MODE=WORKFLOW_ONLY` berarti agent dapat melakukan install/repair/upgrade/verification pada surface C2GRAPH yang diizinkan, tetapi tidak boleh:

- mengubah application/product behavior;
- mengarang objective implementasi;
- mengaktifkan Goal hanya untuk melewati gate;
- menggunakan “sekalian memperbaiki” sebagai alasan memperlebar scope;
- menulis di luar exact workflow-managed set yang diizinkan.

## Entry point

Gunakan `INSTALL-WORKFLOW-ONLY.prompt.md` sebagai executable wrapper. Jangan menjalankan `SCAN-AND-ADOPT.prompt.md` secara langsung untuk user intent workflow-only; direct SCAN-AND-ADOPT berarti `ADOPT_WITH_GOAL`.

## Goal invariance

Workflow-only menjaga logical Goal state. Missing/completed goal bukan error yang harus “diperbaiki”. Goal inventory boleh dibaca sesuai contract tetapi mutation terhadap Goal harus tetap nol pada successful workflow-only finalization.

## Product invariance

`PRODUCT_CHANGES` harus tetap nol. Jika product behavior perlu berubah, hentikan classification workflow-only dan bentuk implementation objective nyata melalui goal-bound path.

## Verification

Repair/install/upgrade harus diverifikasi terhadap manifest, baseline rules, config/schema/path integrity, registered checks, dan invariants yang berlaku. PASS tidak membolehkan product mutation.

## Reporting

Final status harus mempertahankan field yang diminta executable contract, termasuk entry mode dan before/after goal/product mutation status. Jangan menyembunyikan refused/skipped/unverified surface.
