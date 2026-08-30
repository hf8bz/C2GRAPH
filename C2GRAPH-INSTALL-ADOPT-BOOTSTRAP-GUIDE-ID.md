# Panduan Instal, Adopsi, dan Bootstrap C2GRAPH

**English:** [C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE.md](C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE.md)

> Public lifecycle guide untuk tiga formal entry mode C2GRAPH v1.2.0.

## 1. Tiga formal mode

C2GRAPH memiliki tepat tiga mode masuk:

- `ADOPT_WITH_GOAL` — existing non-empty repo + implementation objective nyata;
- `WORKFLOW_ONLY` — existing non-empty repo + hanya install/repair/upgrade/verify C2GRAPH control plane;
- `BOOTSTRAP` — empty/new repo.

`SCAN-AND-ADOPT.prompt.md` adalah **nama executable prompt**, bukan mode keempat bernama `SCAN`.

## 2. ADOPT_WITH_GOAL

Gunakan ketika product/repository behavior memang akan berubah. Existing repo di-scan secara bounded, goal/requirements/evidence dibentuk, lalu mutation hanya mengikuti normal write authority.

## 3. WORKFLOW_ONLY

Gunakan `INSTALL-WORKFLOW-ONLY.prompt.md`. Contract-nya menjaga product behavior dan logical goal state tetap tidak berubah. Ia dapat install/repair/upgrade/verify C2GRAPH control plane dalam scope yang diizinkan tetapi tidak boleh menggunakan workflow maintenance sebagai alasan membuat product change.

Success reporting harus mempertahankan field status yang diwajibkan oleh executable contract, termasuk `GOAL_MUTATIONS: 0` dan `PRODUCT_CHANGES: 0` untuk workflow-only yang sukses.

## 4. BOOTSTRAP

Gunakan untuk repo kosong/baru. Bootstrap dapat membentuk struktur C2GRAPH dan goal-bound initial work sesuai prompt, tetapi tetap tidak menghapus requirement untuk semantic scope, evidence, rollback, dan verification pada mutation berikutnya.

## 5. Install/upgrade integrity

Source tree, distribution manifest, baseline rules, paths, hashes/counts, dan verification harus diperlakukan sebagai kontrak terukur. Locally modified managed files tidak boleh ditimpa diam-diam oleh upgrade yang seharusnya guarded.

## 6. Selection failure

Jangan memilih mode berdasarkan kenyamanan. Jika repo existing tetapi tujuan hanya maintenance governance, gunakan workflow-only. Jika product behavior perlu berubah, buat objective nyata dan gunakan goal-bound path. Jika repo baru, gunakan bootstrap.
