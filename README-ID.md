# C2GRAPH

> **v1.3.2 — hardening Terminal Reconciliation dan Workflow Convergence.**
> C2GRAPH mempertahankan model repository governance dari v1.2.0 sambil membuat completion tetap sticky pada jalur
> resume, menolak reaktivasi terminal same-run dari continuity state yang stale, dan mewajibkan admission berbasis
> evidence untuk pekerjaan successor yang benar-benar baru. Kontrol ini tetap read-only terhadap repository-write authority.

**Versi:** `v1.3.2`  
**Tanggal rilis:** 3 September 2026  
**Rilis publik sebelumnya:** `v1.2.0`, 22 Agustus 2026  
**Predecessor teknis:** `v1.3.1` Workflow Convergence Resume Hardening, 1 September 2026  
**Author:** Hafizh Albanna  
**DOI software:** [`10.5281/zenodo.22259056`](https://doi.org/10.5281/zenodo.22259056)  
**DOI paper rilis:** [`10.5281/zenodo.22259065`](https://doi.org/10.5281/zenodo.22259065)

Bounded evidence read, bounded scanner ingestion, dan boundary model-adapter runtime-scoped/cache-aware termasuk
dalam rilis ini. Product authority tetap diatur oleh rule, goal, evidence, dan write gate C2GRAPH.

Template agar sebuah proyek software bisa dikerjakan oleh agent secara **terukur, terlacak, dan reversibel** - bukan
sekadar dikerjakan.

Template ini **project-agnostic**. Tidak mengasumsikan bahasa, framework, cloud, atau ukuran tim. Semua hal spesifik
proyek diwakili token berkurung ganda - ditulis dalam dokumen ini sebagai `{{ }}` - yang diisi sekali saat adopsi.
Daftar lengkapnya di `PLACEHOLDERS.md`.

---

## Masalah yang diselesaikan

Kegagalan proyek yang dikerjakan agent jarang disebabkan agent yang kurang pintar. Sebab-sebab berikut jauh lebih
sering menjadi penyebabnya:

| Gejala | Akar masalah | Bagian template yang menanganinya |
| --- | --- | --- |
| Aturan menumpuk jadi satu file raksasa, dibaca tiap request | tidak ada pemisahan invarian vs prosedur | `AGENTS.md` tiga lapis |
| Agent yakin padahal salah | tidak ada kontrak evidence | `.agent/runbooks/evidence-contract.md` |
| Discovery mengembalikan hasil yang lebar dan bercampur | tidak ada partisi korpus | `.agent/runbooks/graph-discovery.md`, CORE-18 |
| Evidence besar/minified/log dapat membanjiri read atau failure-nya disalahartikan sebagai file kosong | physical read belum memiliki kontrak berbatas bersama | `scripts/read/core.py`, `.agent/runbooks/bounded-read.md`, CORE-11/12 |
| Tidak bisa membuktikan proyek maju atau mundur | tidak ada guard metric | `prompts/01`, `docs/01` |
| Run kehilangan kontinuitas saat resource habis | tidak ada handoff reserve/resume capsule | `docs/graph-engineering/11-complexity-budget.md`, `.agent/runbooks/budget-resume.md`, CORE-24 |
| Swarm dipakai untuk pekerjaan yang butuh satu konteks utuh | arsitektur dipilih tanpa kriteria | `docs/graph-engineering/10-architecture-selection.md` |
| Node duplikat disatukan salah, seluruh traversal ikut salah | resolusi entitas tanpa bukti dan tanpa jalan balik | `.agent/runbooks/entity-resolution.md` |
| Pelajaran yang sama diulang tiap minggu | lesson tidak terstruktur dan tidak bisa dikueri | `.agent/lessons.jsonl`, `prompts/02` |
| Sesudah CLI mengompaksi, agent mengulang pekerjaan yang sudah selesai | state kerja hanya hidup di percakapan | `.agent/runbooks/context-compaction.md`, CORE-25 |
| Implementasi macet dan menanyakan langkah berikutnya di tiap tahap | objective-nya sebuah pesan, bukan kontrak yang tahan lama | `.agent/runbooks/goal-contract.md`, CORE-26 |
| Laporannya terbaca lebih baik daripada jalannya run yang sebenarnya | kepatuhan tidak pernah teramati per giliran | `.agent/runbooks/workflow-discipline.md`, CORE-27 |
| Agent mengarang prosedur untuk tool yang sudah dipakai tim | pemakaian nyata tidak pernah ditangkap dari call site | `.agent/runbooks/tool-usage-capture.md`, CORE-28 |
| Dokumentasi dan test berselisih, dan agent memercayai prosanya | tidak ada urutan antara sumber yang jujur dan yang aspiratif | `.agent/runbooks/test-to-workflow.md`, CORE-29 |
| Context terstruktur berulang boros token atau tidak cocok dengan model efektif | satu representasi dipaksakan ke semua model | `.agent/runbooks/context-representation.md`, ACRP |
| Recall failure/lesson teracun tetap actionable saat insiden | containment kognitif disamakan dengan mutasi repository | `.agent/runbooks/cognitive-emergency-authority.md`, CEA |
| Rilis mobile keluar tanpa jalan kembali | asumsi web diterapkan pada artefak yang tidak bisa dibalik | ketiga runbook mobile, CORE-31 |

---

## Tiga lapis konteks

```
LAPIS 1  AGENTS.md              <= router_max_lines (220). Dibaca SETIAP request. Hanya invarian + routing.
LAPIS 2  .agent/scopes/*.md     Dimuat saat lane tertentu aktif. Aturan per domain.
LAPIS 3  .agent/runbooks/*.md   Dimuat saat prosedur tertentu dijalankan. Langkah detail.
```

Alasannya adalah bounded context, bukan angka token universal. Rulebook monolit memuat prosedur yang tidak
relevan pada setiap request; router membatasi always-loaded governance ke `router_max_lines` (220), sedangkan
scope/runbook dimuat saat relevan. Token dan cache-hit effect bergantung tokenizer, host, dan prompt layout;
ukur angka aktual proyek melalui `docs/graph-engineering/04-context-budget.md` sebelum membuat claim biaya atau cache.

---

## Isi paket

Total 239 file.

```
AGENTS.md                       router generik, CORE-01..CORE-40 + routing table 46 baris
START-HERE.md                   pemilih 3 prompt pintu masuk
SCAN-AND-ADOPT.prompt.md        repo existing + adopsi pengemisi goal, FASE 0-8
INSTALL-WORKFLOW-ONLY.prompt.md repo existing + workflow/control-plane saja; goal opsional, tidak emit goal
BOOTSTRAP-NEW-REPO.prompt.md    repo baru/kosong + bootstrap pengemisi goal, FASE 0-8
PLACEHOLDERS.md                 54 token + artinya + contoh
CHANGELOG.md                    log rilis, kebijakan stabilitas ID aturan, hasil audit per rilis
CHECKLIST.md                    checklist adopsi + checklist ketahanan + 8 tingkat kematangan
.gitattributes                  normalisasi line ending (eol=lf)
.agent/config.yml               profil proyek: root, stack, mobile, partisi, exclusion, ambang
.agent/scopes/                  10 scope contoh + _TEMPLATE.md
.agent/runbooks/                40 runbook + _TEMPLATE.md
.agent/runbooks/generated/      tujuan runbook yang dibangkitkan dari pemakaian tool nyata
.agent/graph/schema/            nodes.md + edges.md + example.jsonl
.agent/lessons.jsonl            korpus pelajaran (kosong) + lessons.schema.md
.agent/runs/                    log eksekusi agent + runs.schema.md
.agent/state/                   handoff, cursor, dan catatan disiplin (ditulis saat runtime)
.agent/goals/                   kontrak goal yang diterbitkan (ditulis saat runtime)
.agent/requirements.schema.md   skema requirement (dipakai BOOTSTRAP, berguna juga di repo lama)
.agent/handoff.schema.md        skema checkpoint: keenam belas bagian dan maknanya
.agent/goals.schema.md          skema goal: field, keadaan, dan penggantian
.agent/toolmap.schema.md        skema peta tool: provenance, confidence, konfirmasi
docs/graph-engineering/         26 dokumen hasil + _decisions.md + references.md
docs/guides/                    panduan perilaku terverifikasi per lane (ditulis saat runtime)
prompts/                        12 prompt: bertahap 00-06, 07 latihan, 08 goal, 97 budget-resume, 98 handoff, 99 resume
eval/retrieval/golden.example.jsonl  contoh golden set, disalin jadi golden.jsonl
eval/rubrics/_TEMPLATE.md       rubrik penilaian, bukti non-kompensable
scripts/scan/                   4 scanner read-only: tooling, census, plan, redact untuk config rawan-secret
scripts/README.md               kontrak script, language-agnostic
```

---

## Lapisan ketahanan

Enam kemampuan, masing-masing dengan runbook yang menjelaskan caranya, dokumen yang menjelaskan alasannya, dan gate
yang membuktikan ia benar-benar terjadi.

### 1. Handoff kompaksi

Ketika CLI atau IDE mengompaksi percakapannya, state kerja lenyap dan agent diam-diam mengulang pekerjaan. Lima
kelas deteksi menangkap sinyalnya - pengumuman eksplisit, rasio konteks, bukti senyap seperti kenari yang hilang,
sebuah event host, atau penghitung langkah. Checkpoint dipersist **sebelum** ada yang hilang hanya bila exact state
action post-3.W ada di `ACTUAL_WRITE_SET`, path-scoped authorized, dan fresh. Selain itu konten yang sama tetap
`PENDING_HANDOFF` di evidence session/host dan TARGET_ROOT tetap byte-read-only; bila evidence eksternal itu tidak
bisa melewati compaction, run berhenti alih-alih membuat state tanpa unit rollback. Sesi persisted berikutnya
merehidrasi dari `.agent/state/HANDOFF.md`, menggemakan apa yang diyakininya, melaporkan `DIVERGENSI` apa pun, lalu
menunggu.

Checkpoint-nya mendarat di graph sebagai node `Checkpoint` dengan edge `CHECKPOINT_OF`, `RESUMES`, dan `SUPERSEDES`,
sehingga run yang dilanjutkan bisa dilacak ke run yang diteruskannya alih-alih tampak seperti run yang baru. Baca
`docs/graph-engineering/12-compaction-handoff.md`; latih dengan `prompts/07`.

### 2. Kontrak goal

Product implementation bersifat goal-bound. Ketika requirement lengkap, `prompts/08-goal-emit.prompt.md` memeriksa
readiness gate tujuh syarat lalu menerbitkan blok siap-tempel untuk `/goal`: kondisi selesai, urutan tahap, gate
tahap, tindakan terlarang, dan kondisi berhenti. Setiap command Python bawaan memakai launcher `<C2PY>` yang
di-resolve dari host: POSIX memakai `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`, sedangkan Windows memakai
`<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd`. Jangan mengasumsikan literal `python3`. Resolve ulang setelah CLI/sesi
restart.

Sebelum product work, `<C2PY> scripts/goal/gate.py --require-active` harus lulus.

`INSTALL-WORKFLOW-ONLY.prompt.md` adalah pengecualian yang sempit: prompt ini memasang atau memperbaiki control
plane C2GRAPH di repo existing sambil mempertahankan state goal `absent`, `empty`, `active`, `terminal_only`, atau
`inactive`, dan tidak emit goal. Mode ini tidak boleh mengubah behaviour produk. Objective kemudian mendapat goal
sendiri melalui `prompts/08-goal-emit.prompt.md`; goal completed tidak pernah diaktifkan kembali hanya demi
instalasi workflow.

Readiness gate ketat dengan sengaja. Ketika gagal, yang didapat adalah butir yang kurang, bukan goal, karena goal di
atas requirement belum lengkap mengubah tebakan menjadi instruksi yang berdiri terus.

### 3. Disiplin workflow

Aturan yang dibaca tidak sama dengan aturan yang diikuti. Sebuah PREFLIGHT sebelum bekerja dan sebuah STATUS LINE
sesudah tiap giliran membuat kepatuhan teramati, tujuh aturan urutan D1-D7 membuatnya bisa dicek, lima belas kode
pelanggaran V01-V15 membuatnya bisa dihitung, dan `discipline.jsonl` membuatnya bisa ditinjau. Pelanggaran yang
disebabkan host dicatat sebagai celah tooling, bukan sebagai kesalahan model.

### 4. Mobile: Android dan iOS

Satu scope mobile generik tidak cukup untuk kedua platformnya, jadi template ini mengirim dua scope platform
(`AND-01..AND-18`, `IOS-01..IOS-20`) ditambah tiga runbook yang mencakup build dan rilis, QA perangkat terhadap
matriks lima slot, dan pengiriman ke toko. Batasan yang menatanya adalah ketidakterbalikan: penggunalah yang
memegang binernya, jadi jalur rollback disebut sebelum rilisnya, tidak pernah sesudahnya.

### 5. Penangkapan cara pakai tool

Tiga belas kelas sumber dipindai untuk mengetahui bagaimana proyek ini sebenarnya memanggil tool-toolnya, dan
runbook dibangkitkan **secara otomatis** dari call site-nya, tiap langkah mengutip `path:span`. Entri yang
destruktif, ambigu, berconfidence rendah, dan berduplikasi tujuan tidak dibangkitkan - semuanya menjadi pertanyaan
FASE 3 yang diajukan satu per pesan. Tool terpasang dengan nol call site juga dilaporkan.

### 6. Test dan rencana menjadi dokumen

Setiap file test dibaca dan dikonversi menjadi kewajiban dengan confidence tinggi; dokumen dibaca belakangan dan
dibatasi lebih rendah, karena test adalah apa yang dilakukan sistemnya dan dokumen adalah apa yang pernah diniatkan
seseorang. Ketika keduanya berselisih, keduanya dicatat dan manusia yang memutuskan. Untuk repo baru, setiap elemen
rencana dikonversi ke format dokumen yang benar-benar dipakai jenis repositorinya, dilacak di sebuah buku besar
konversi agar tak ada yang diam-diam terbuang dan tak ada yang dikarang untuk mengisi sel tabel.

---

## Artefak yang dibuat saat runtime

Kelima belas file di bawah **tidak** ikut di paket ini dan itu memang benar: masing-masing lahir dari hasil
pengukuran, bukan dari template. Rujukan ke file-file itu di dokumen lain karenanya bukan referensi menggantung.

Kolom `Produsen logis` adalah provenance, **bukan izin write**. Setiap target write tetap memerlukan semantic scope,
exact row `ACTUAL_WRITE_SET` post-3.W, path-scoped authority, dan freshness terkini. Karena itu suatu artifact dapat
tetap session-only atau tidak pernah dibuat sepanjang run.

| File | Produsen logis | Isi |
| --- | --- | --- |
| `.agent/tooling.yml` | inventory GATE 0; persistence post-3.W opsional | discovery statis + evidence eksekusi yang disetujui + batas klaim |
| `.agent/draft/**` | draf logis FASE 2/1; normalnya evidence sesi | artifact konfigurasi usulan opsional |
| `.agent/requirements.jsonl` | evidence bootstrap FASE 1; persistence post-3.W opsional | requirement hasil ekstraksi |
| `eval/retrieval/golden.jsonl` | `prompts/05-retrieval-eval.prompt.md` | salinan terisi dari `golden.example.jsonl` |
| `docs/graph-engineering/_ledger.jsonl` | FASE 5 `SCAN-AND-ADOPT.prompt.md` | ledger kalimat rulebook lama |
| `.agent/toolmap.jsonl` | FASE 1.13 / 1.6 | pemakaian tool yang ditangkap beserta provenance dan confidence |
| `.agent/state/HANDOFF.md` | `.agent/runbooks/context-compaction.md` | checkpoint-nya, enam belas bagian |
| `.agent/state/handoff.jsonl` | runbook yang sama | ledger event checkpoint/resume/status append-only |
| `.agent/state/CURSOR.json` | runbook yang sama | penunjuk resume terkecil, selalu ditulis paling akhir |
| `.agent/state/discipline.jsonl` | `.agent/runbooks/workflow-discipline.md` | satu baris persisted per giliran kerja berotorisasi; giliran pra-authority tetap `PENDING_DISCIPLINE` di evidence sesi |
| `.agent/goals/goals.jsonl` | `.agent/runbooks/goal-contract.md` | indeks goal beserta status dan penggantiannya |
| `.agent/goals/GOAL-<nnn>.md` | runbook yang sama | blok siap tempel, sebagaimana diserahkan |
| `.agent/runbooks/generated/<tool>-<procedure>.md` | `.agent/runbooks/tool-usage-capture.md` | prosedur yang dibangkitkan dari call site nyata |
| `docs/guides/<lane>-verified-behaviour.md` | `.agent/runbooks/test-to-workflow.md` | terkonfirmasi, tanpa perlindungan, kontradiksi, ditangguhkan |
| `docs/product/_conversion-ledger.md` | FASE 5.9 `BOOTSTRAP-NEW-REPO.prompt.md` | satu baris per elemen rencana yang dikonversi |

Menyertakan semuanya sebagai file kosong justru berbahaya: file kosong yang valid secara sintaks akan lolos gate dan
menyembunyikan fakta bahwa pengukurannya belum pernah dijalankan.

---

## Pintu masuk

Baca `START-HERE.md`. Ada **3 prompt pintu masuk**; pilih berdasarkan state repo dan hasil yang diinginkan.

| Kondisi repo / hasil | Prompt |
| --- | --- |
| repo existing, adopsi berakhir dengan goal implementasi | `SCAN-AND-ADOPT.prompt.md` |
| repo existing, hanya pasang/perbaiki workflow C2GRAPH; active goal tidak wajib ada | `INSTALL-WORKFLOW-ONLY.prompt.md` |
| masih kosong / baru | `BOOTSTRAP-NEW-REPO.prompt.md` |

Ketiganya mempertahankan disiplin scan, decision, dan verification yang sama. Wrapper workflow-only mendelegasikan
ke SCAN-AND-ADOPT dengan `ENTRY_MODE=WORKFLOW_ONLY`; FASE 8-nya mempertahankan goal plane dan tidak emit goal. Dua
jalur lainnya memakai finalisasi pengemisi goal setelah readiness lulus.

## Quickstart

1. Ekstrak distribusi C2GRAPH **di luar** repositori sasaran. Jangan menyalin seluruh paket ke
target hanya agar prompt atau script tersedia. Anggap direktori hasil ekstrak sebagai `C2GRAPH_SOURCE_ROOT` dan
proyek sebagai `TARGET_ROOT`.
2. Berikan tepat satu prompt pintu masuk dari tabel di atas kepada agent. FASE 0 menjalankan static
discovery tanpa mutasi dan **preliminary rollback-basis scan** dari `C2GRAPH_SOURCE_ROOT`; scan itu sengaja
menghasilkan `ROLLBACK_AUTHORITY=pending_write_plan`. Setelah keputusan FASE 3, agent membangun exact
`ACTUAL_WRITE_SET` lalu menjalankan preflight dengan `--plan-op ACTION:PATH`, atau `--plan-empty` bila tidak ada
byte target yang perlu berubah. Existing shipped path yang identik dapat memakai `distribution_restore`; collision
berbeda yang tidak ada di plan tidak relevan.
3. Pertahankan FASE 0-2 target-byte read-only: jangan membuat `AGENTS.v2.md`, ledger failure/tooling,
draft, generated docs, cache, lockfile, dan jangan menjalankan package-manager runner atau tool project-local.
Persistence sesudah FASE 3 bersifat path-scoped: generic authority untuk satu path tidak mengotorisasi audit/state
path lain, dan persistence-only artifact boleh tetap pending tanpa memblokir semantic installation work.
4. Biarkan prompt pintu masuk menginstansiasi template. Jika `AGENTS.md` existing perlu dimigrasikan,
kandidat `AGENTS.v2.md` baru dibuat hanya setelah FASE 3.W memasukkan exact path tersebut ke `ACTUAL_WRITE_SET` dan
path-scoped authority/freshness-nya lulus; file lama tetap tidak berubah sampai lossless swap gate lulus.
5. Jalankan `prompts/00-baseline-audit.prompt.md` (read-only).
6. Jalankan `prompts/01-measurable-loop.prompt.md`. **Jangan lewati nomor 01.** Tanpa metrik,
semua langkah setelahnya tidak bisa dibuktikan.
7. Sisanya mengikuti urutan di `CHECKLIST.md`.

---

## Prinsip yang tidak bisa dinegosiasi

1. **Instrumentasi sebelum optimasi.** Angka perbaikan yang tidak punya baseline adalah karangan.
2. **Lossless saat restrukturisasi.** Memindahkan aturan tidak boleh menghilangkan kewajiban.
3. **Ragu itu dicatat, bukan ditebak.** `needs_human_decision` selalu lebih murah dari asumsi.
4. **Setiap perubahan punya unit rollback bernama.**
5. **Gate keras tidak pernah dipangkas demi kerapian.**
6. **Setiap output penting bisa dilacak** ke objective, plan, artefak, sumber, jalur graph,
putusan evaluator, dan catatan eksekusi berbatas. Asal gagasan dicatat di `docs/graph-engineering/references.md`.

## Glosarium

- sliceSpecificity adalah rasio hasil yang terpakai dibagi hasil yang dikembalikan satu kueri.
- noiseComposition adalah rincian asal hasil yang dibuang: modul lain, legacy, generated,
fixture, vendor.
- fallbackRate adalah porsi tugas yang turun dari graph ke pencarian teks berpartisi.
- Golden set adalah daftar pertanyaan penemuan beserta jawaban benarnya, dipakai mengukur
Precision@5.
- Partisi korpus adalah pembagian repo menjadi wilayah yang bisa dikueri terpisah.
- Bidang graph adalah lapisan artefak `.agent/*.jsonl` yang menyimpan objektif, run, bukti,
dan pelajaran.
- Ratchet adalah putaran perbaikan yang hanya menyimpan perubahan bila angkanya membaik.
- Tangga fallback adalah urutan alat penemuan: graph, pencarian berpartisi, lalu pembacaan
langsung.

## Prompt tambahan

- `prompts/02-lessons-corpus.prompt.md` menyusun korpus pelajaran menjadi
`docs/graph-engineering/02-lessons.md`.
- `prompts/06-operations.prompt.md` menyusun catatan operasi menjadi
`docs/graph-engineering/06-operations.md`.
- `prompts/07-compaction-drill.prompt.md` mengukur bagaimana CLI atau IDE spesifikmu berperilaku saat
mengompaksi. Jalankan sekali per host, sebelum kamu bergantung pada mekanisme handoff-nya, bukan sesudahnya.
- `prompts/08-goal-emit.prompt.md` menjalankan gate kesiapan dan menerbitkan blok goal siap tempel.
- `prompts/98-compaction-handoff.prompt.md` adalah pasangan yang benar-benar kamu tempel sehari-hari:
98A menulis checkpoint-nya, 98B merehidrasi darinya.

## Urutan baca untuk dokumen baru

| Dokumen | Menjawab |
| --- | --- |
| `docs/graph-engineering/12-compaction-handoff.md` | apa yang selamat melewati kompaksi, apa yang tidak, dan kenapa perbaikan yang tampak jelas justru gagal |
| `docs/graph-engineering/13-goal-contract.md` | kenapa goal adalah kontrak alih-alih rencana, dan kenapa gate-nya ketat |
| `docs/graph-engineering/14-tool-usage-map.md` | kenapa call site mengalahkan dokumentasi sebagai bukti praktik |
| `docs/graph-engineering/15-test-workflow-map.md` | kenapa test dibaca sebelum dokumen, dan apa yang tidak dibuktikan tiap kelas test |
| `docs/graph-engineering/16-mobile-platform.md` | kenapa ketidakterbalikan, bukan sintaks platform, yang menjadi batasan penatanya |
| `docs/graph-engineering/17-workflow-discipline.md` | jurang antara buku aturan dan run yang berjalan, dan apa yang menutupnya |
| `docs/graph-engineering/18-scan-automation.md` | bagian scan mana yang diukur script, dan bagian mana yang rusak kalau diserahkan ke script |

Masing-masing menyatakan batasnya di bagian terakhir. Dokumen yang hanya mendaftar manfaat adalah materi pemasaran,
dan ia mengajari agent untuk mengklaim berlebihan dalam register yang sama.

## Baca evidence terbatasi

File text besar, minified, sangat lebar, atau belum dikenal masuk context melalui `scripts/read/core.py`. Reader ini
stateless dan repository-read-only; ia menerapkan ceiling line, byte UTF-8, dan karakter physical-line serta
mengeluarkan cursor v2 generation-bound berisi `(line, char)`, offset byte source exact, digest generasi source,
digest profil, dan sequence. Setelah tiga window sequential, jalur default mewajibkan strategy review sebelum blind
pagination berikutnya. Path rawan-secret tetap memakai
`scripts/scan/redact.py`. Lihat `.agent/runbooks/bounded-read.md` dan `docs/graph-engineering/19-bounded-read.md`.


## Progress Liveness dan Self-Audit Berbatas

Control plane sekarang memisahkan activity, diagnostic progress, dan semantic progress berbasis evidence.
Repeated failure, checkpoint yang tidak berubah, atau soft threshold terkonfigurasi memicu diagnosis awal;
hard boundary default adalah delapan jam (`progress_hard_seconds: 28800`). Lane audit read/reason/review
bersifat advisory (`canonical_write=false`), dan watchdog tidak pernah membuat repository-write authority.
Lihat `.agent/runbooks/progress-liveness.md`.

## Hardening historis v1.2.0 - 19 Agustus 2026

C2GRAPH v1.3.2 mempertahankan strict numeric validation untuk Progress Liveness & Bounded Self-Audit.
Boolean/numeric string JSON tidak dapat menyamar sebagai metric, fractional integer counter fail closed, dan
deadline/confidence wajib finite serta berada dalam range. Evidence progress saat ini adalah **33/33 directed
scenario + 65.536/65.536 declared abstract state dengan 0 invariant violation**. Workflow Convergence adalah
layer governance read-only tambahan dan tidak mengubah canonical write-authority semantics.



## Terminal Reconciliation dan admission successor - v1.3.2

Cursor durable bersifat deskriptif, bukan terminal truth. Sebelum automatic resume, C2GRAPH memeriksa exact AgentRun;
`COMPLETED` tidak dapat dibuka ulang oleh cursor `open` yang stale. Run/task ID berbeda bukan semantic progress.
Terminal successor memerlukan `terminal_successor`, digest evidence exact, `basis_claim_id` canonical yang unik global,
dan predecessor yang terverifikasi independen. Claim ID hanya boleh dikonsumsi sekali sepanjang handoff ledger, jadi
perubahan class, semantic delta, path evidence, model, reviewer, atau sesi tidak dapat membuat successor baru dari
claim yang sama. Admission successor tetap read-only dan WAJIB direvalidasi di bawah state lock yang sudah ada tepat
sebelum append checkpoint. Lihat `.agent/runbooks/workflow-convergence.md` dan `scripts/handoff/terminal_reconcile.py`.

