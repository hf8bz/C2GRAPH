# Referensi Konfigurasi C2GRAPH

**English:** [C2GRAPH-CONFIGURATION-REFERENCE.md](C2GRAPH-CONFIGURATION-REFERENCE.md)

> Companion Bahasa Indonesia untuk `C2GRAPH-CONFIGURATION-REFERENCE.md`. `.agent/config.yml` tetap menjadi owner runtime/configuration; dokumen ini bersifat deskriptif.

## 1. Prinsip konfigurasi

Tunable value ditempatkan pada satu owner agar threshold tidak tersebar di banyak dokumen. Konfigurasi tidak boleh mengganti semantic scope, exact write authority, rollback requirement, fresh pre-state, atau verification requirement.

## 2. Keluarga konfigurasi yang dipublikasikan

Public Reference mengekspos konfigurasi untuk:

- project/stack binding;
- command dan path;
- graph/search/edit tooling;
- mobile binding dan QA matrix;
- routing/read/retrieval boundaries;
- agent output dan concurrency;
- batch control;
- lessons/failure recall;
- ratchet loop;
- graph-write/tool-call budgets;
- compaction/handoff/resume;
- interactive question policy;
- tool discovery/execution policy;
- provenance;
- compliance mapping;
- rule preservation.

## 3. Project/tool bindings

Nama field dan token tidak diterjemahkan. Surface yang dipublikasikan mencakup antara lain `name`, `root`, `shell`, `primary_language`, `package_manager`, `orm`, `database`, `build`, `test`, `lint`, `deploy`, `migrations`, `tracker`, `product_doc`, `design_doc`, `production`, `staging`, `graph`, `search`, `find`, `struct_edit`, `loc_count`, `json_query`, `work_graph`, Android module binding, iOS scheme binding, serta `qa_matrix`.

Placeholder template bukan value instantiated yang valid ketika gate mensyaratkan project binding nyata.

## 4. Threshold versus invariant

Bedakan:

- **tunable threshold** → boleh berubah melalui owner konfigurasi yang sah;
- **semantic invariant** → tidak boleh dilemahkan hanya dengan mengganti angka config;
- **generated measurement** → output observasi, bukan policy;
- **runtime state** → state session/run, bukan konfigurasi permanen.

Contoh: budget, concurrency, retrieval ceiling, recall age, atau checkpoint interval dapat bersifat konfigurabel; tetapi exact `ACTUAL_WRITE_SET` dan requirement untuk fresh pre-state bukan sekadar threshold convenience.

## 5. Anti-drift

Jika nilai yang sama muncul di prose, script, dan config, satu sumber harus ditetapkan sebagai owner dan surface lain memvalidasi/merujuknya. Verification seharusnya gagal ketika angka yang dipublikasikan tidak lagi cocok dengan source owner.

## 6. Referensi silang

- `C2GRAPH-REFERENCE-ID.md` untuk rule/gate/threshold yang dipublikasikan;
- `C2GRAPH-CONTROL-PLANE-CATALOG-ID.md` untuk posisi config di control plane;
- `C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md` untuk verifier yang menjaga drift.
