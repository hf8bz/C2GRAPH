# C2GRAPH

**English:** [README.md](README.md)

### Convert to Graph Engineering

**Tata kelola tingkat repository untuk AI coding agent yang bekerja dalam horizon panjang.**

C2GRAPH membuat pekerjaan software berbasis agent lebih **bounded, measurable, traceable, resumable, reversible, dan auditable** dengan memindahkan execution state penting dari conversation history sementara ke kontrak repository yang eksplisit.

[![Release candidate](https://img.shields.io/badge/candidate-v1.4.19-lightgrey)](https://github.com/hf8bz/C2GRAPH)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Paper DOI](https://img.shields.io/badge/Paper_DOI-10.5281%2Fzenodo.22259065-blue)](https://doi.org/10.5281/zenodo.22259065)
[![Software DOI](https://img.shields.io/badge/Software_DOI-10.5281%2Fzenodo.22259056-blue)](https://doi.org/10.5281/zenodo.22259056)
[![Historical source checks v1.3.2](https://img.shields.io/badge/historical_checks-v1.3.2-lightgrey)](https://github.com/hf8bz/C2GRAPH/releases/tag/v.1.3.2)

**Versi 1.4.19 · Kandidat publikasi · 29 Oktober 2026**

**Rilis publik sebelumnya:** v1.3.2 · 2 September 2026  
**Baseline source stabil:** v1.4.19 · 14 Oktober 2026

[Previous public release](https://github.com/hf8bz/C2GRAPH/releases/tag/v.1.3.2) ·
[Previous public paper](https://doi.org/10.5281/zenodo.22259065) ·
[Previous software archive](https://doi.org/10.5281/zenodo.22259056) ·
[Indeks Dokumentasi](C2GRAPH-PUBLIC-DOCS-INDEX-ID.md) ·
[Ringkasan Rilis Sebelumnya](C2GRAPH-V1.3.2-RELEASE-OVERVIEW-ID.md) ·
[Terminal Reconciliation](C2GRAPH-TERMINAL-RECONCILIATION-GUIDE-ID.md) ·
[English](README.md) ·
[Panduan Fitur Baru](C2GRAPH-V1.4.19-FEATURE-GUIDES-ID.md)

---

## Mengapa C2GRAPH ada

Coding agent modern dapat menulis code dengan baik.

Masalah yang lebih sulit muncul ketika pekerjaan berlangsung lama, lintas session, lintas model, lintas tool budget, atau melibatkan repository besar.

Agent masih dapat:

* kehilangan objective setelah context compaction;
* menemukan ulang repository fact yang sebenarnya sudah diketahui;
* mencampur evidence dari aplikasi berbeda dalam monorepo;
* mengulang approach yang sebelumnya sudah gagal;
* mengubah inference menjadi implementation requirement tanpa provenance;
* memanggil tool melalui model/runtime protocol yang tidak sesuai;
* memperluas mutation scope karena adjacent change terlihat berguna;
* kehilangan rollback context;
* menganggap cache hit sebagai correctness evidence;
* menganggap aktivitas berulang sebagai semantic progress;
* menghidupkan kembali run yang sebenarnya sudah `COMPLETED` karena CURSOR/HANDOFF stale;
* atau melaporkan completion dengan evidence yang tidak benar-benar membuktikan claim.

C2GRAPH memperlakukan banyak masalah ini bukan sebagai kegagalan misterius model, tetapi sebagai **governance problem dan state-transition problem yang dapat dibuat eksplisit, diperiksa, dan divalidasi**.

Prinsip pusatnya adalah:

> **Knowledge tidak otomatis menjadi authority.**

Search result bukan permission untuk menulis.

Goal bukan arbitrary write authority.

Cache hit bukan correctness evidence.

Valid tool call bukan bukti bahwa tool yang dipilih secara semantik benar.

Rollback mechanism tidak memperluas semantic scope.

Checkpoint tidak menciptakan permission untuk melakukan mutation.

Continuity state tidak dapat menghidupkan kembali terminal execution truth.

Dan confidence bukan verification.

---

# Apa itu Graph Engineering?

**C2GRAPH berarti Convert to Graph Engineering.**

Graph Engineering bukan sekadar menambahkan graph database ke AI system.

Graph Engineering berarti membuat hubungan penting dalam engineering work menjadi explicit state yang dapat diperiksa.

Contoh sederhana:

```text
Objective
   │
   ▼
 Goal ───────────────► Requirements
   │                       │
   ▼                       ▼
 Task ─────► Run ─────► Artifact
   │          │             │
   │          ▼             ▼
   │       Evidence ─────► Claim
   │          │             │
   ▼          ▼             ▼
Decision    Metric       Evaluation
   │
   ▼
Mutation ──► Verification
   │
   ├────────► Failure ──► Lesson
   │
   └────────► Checkpoint ──► Resume
```

Tujuannya bukan memasukkan semuanya ke graph.

Tujuannya adalah membuat cukup banyak hubungan kerja eksplisit sehingga session berikutnya, model berikutnya, worker berikutnya, reviewer, atau manusia dapat menjawab:

* Apa objective aktifnya?
* Apa requirement yang benar-benar berlaku?
* Apa yang sudah dilakukan?
* Evidence apa yang mendukungnya?
* Apa yang masih ambiguous?
* Apa yang boleh berubah berikutnya?
* Apa exact write authority yang tersedia?
* Apa recovery basis untuk mutation tersebut?
* Apa yang gagal?
* Apa lesson yang masih relevan?
* Di mana execution harus resume?
* Apakah run masih resumable atau sebenarnya terminal?
* Evidence apa yang membenarkan successor baru?
* Evidence apa yang membuktikan completion?

Graph di C2GRAPH adalah representasi hubungan kerja dan evidence.

Graph bukan authority dengan sendirinya.

---

# Prinsip desain utama

C2GRAPH dapat diringkas melalui empat prinsip utama.

### 1. Bounded Autonomy

Agent dapat bekerja secara autonomous, tetapi expansion dibatasi pada context, retrieval, graph traversal, mutation scope, retry loop, batching, concurrency, recovery, dan resource use.

Autonomy bukan izin untuk terus bekerja tanpa stop condition.

### 2. Evidence-Bound Authority

Action penting diikat ke evidence, scope, provenance, rollback, freshness, dan verification eksplisit, bukan model confidence.

Evidence dapat membantu menentukan authority.

Evidence tidak otomatis menciptakan authority.

### 3. Durable Operational Learning

Failure, correction, lesson, discovery, decision, checkpoint, dan continuation state dapat bertahan melewati session boundary tanpa berubah menjadi permanent unquestioned truth.

State yang durable tetap memiliki provenance, freshness, lifecycle, dan applicability boundary.

### 4. Compatibility Without Authority Leakage

Perbedaan provider, model, tool-call format, reasoning state, routing, dan cache tetap berada di host/runtime adapter boundary.

Compatibility layer tidak boleh diam-diam menjadi repository authority.

---

# Perubahan dari v1.3.2 ke v1.4.19

Bagian ini menjelaskan source lokal **v1.4.19** yang dibekukan. Penjelasan teknis dan identitas DOI v1.3.2 tetap menjadi catatan historis. Kandidat ini belum dipublikasikan di GitHub dan belum memiliki DOI baru.

- **Assurance aplikasi:** skenario terikat subject, role, perubahan state, tes negatif dan kewajiban test-to-workflow.
- **Kapabilitas domain universal:** discovery domain/kapabilitas bersifat pilihan; invariant akuntansi dan perilaku temporal tetap memerlukan bukti proyek.
- **Keamanan resource:** fixture terbatas, anggaran read/output/proses, temporary path yang dimiliki dan prosedur snapshot/restore.
- **QA keamanan:** pemeriksaan izin akses, isolasi role/tenant dan pengujian aktif yang memiliki scope jelas.
- **Resume dan otomasi:** hook yang lebih eksplisit, terminal continuity, orkestrasi audit dan ekspor non-destruktif.

Baca [Panduan Fitur Baru](C2GRAPH-V1.4.19-FEATURE-GUIDES-ID.md); gunakan file kanonis yang dirujuk untuk tindakan operasional. Paket panduan ini tidak berisi laporan audit atau evidence pengujian.

**Catatan arsip:** Badge Paper DOI dan Software DOI di atas masih merujuk pada publikasi **v1.3.2**, bukan DOI v1.4.19.

---

# Riwayat: perubahan dari v1.2.0 ke v1.3.2

v1.3.2 adalah rilis Version 1 yang backward-compatible terhadap rilis publik sebelumnya, v1.2.0.

Canonical mutation predicate tidak diganti.

Repository mutation tetap bergantung pada semantic scope, exact `ACTUAL_WRITE_SET`, rollback readiness, fresh expected pre-state, dan verification.

Line 1.3.x menambahkan convergence dan terminal-state boundary di atas model authority tersebut.

### CORE-40 Workflow Convergence

C2GRAPH sekarang membedakan **semantic progress** dari procedural activity.

Recovery terhadap semantic frontier yang tidak berubah dibatasi:

```text
NO_PROGRESS_OBSERVED
        │
        ▼
DIAGNOSE_ONCE
        │
        ▼
masih tidak ada semantic delta?
        │
        ▼
STOP_BUSY_LIVELOCK
```

Mengganti run ID, task ID, model, reviewer, session, wording, atau mengulang audit bukan semantic progress dengan sendirinya.

Re-planning hanya diterima melalui closed invalidator set yang dideklarasikan.

### Sticky completion dan PASS-only verification reuse

Completion bersifat sticky terhadap resume/recovery.

Continuity state yang stale atau partial tidak boleh membuka kembali completed frontier secara diam-diam.

Verification reuse hanya dapat dilakukan jika verification identity sama secara exact dan prior result adalah `PASS`.

Changed identity, contradictory state, atau unsupported status membutuhkan fresh verification atau fail closed.

### Cross-plane terminal reconciliation

v1.3.2 menambahkan terminal boundary antara execution state dan continuity state.

Jika exact AgentRun sudah `COMPLETED`, CURSOR/HANDOFF stale yang masih `open` tidak dapat menghidupkan kembali run yang sama.

Terminal execution truth mengalahkan stale descriptive continuity untuk same-run resume.

Jika predecessor `COMPLETED` membawa `evaluation_id`, verified completion membutuhkan tepat satu evaluation untuk run yang sama dengan verdict `kept`.

Missing, duplicate, wrong-run, contradictory, atau non-`kept` evaluation state gagal secara fail-closed.

### Evidence-bound terminal successor admission

Run ID baru bukan progress dengan sendirinya.

Successor setelah terminal completion membutuhkan metadata `terminal_successor` yang mengikat:

* predecessor run;
* predecessor evaluation jika ada;
* successor class yang diizinkan;
* semantic delta yang bermakna;
* `basis_ref`;
* exact `basis_sha256`;
* canonical `basis_claim_id`.

Semantic claim identity hanya dapat dikonsumsi satu kali secara global di handoff ledger.

Mengubah class, delta, model, reviewer, session, run/task ID, evidence path, atau menyalin evidence yang sama tidak menciptakan successor permit baru.

### Evidence-path dan alias hardening

Basis evidence untuk terminal successor harus merupakan regular UTF-8 file yang berada di bawah repository root dan memiliki digest exact.

Claim harus muncul sebagai exact token.

Control/continuity/audit paths seperti:

```text
.agent/state/**
.agent/runs/**
.agent/graph/**
backups/**
```

atau symlink alias menuju path tersebut tidak dapat dipakai sebagai successor basis.

Continuity bookkeeping tidak boleh menjadi evidence yang membenarkan continuation bookkeeping itu sendiri.

### Resume mode dan successor mode dipisahkan

v1.3.2 membedakan dua pertanyaan:

```text
Apakah existing run ini boleh resume?
```

serta:

```text
Apakah verified terminal predecessor ini
boleh menerima successor baru?
```

Default `handoff/terminal_reconcile.py` adalah resume gate.

`--successor-json FILE` adalah successor-admission mode.

Karena itu terminal predecessor yang valid memang harus ditolak untuk same-run resume, tetapi masih dapat mengizinkan successor yang valid dan independently evidenced.

### Lock/fence revalidation sebelum append

Terminal admission helper bersifat read-only.

Successful preflight bukan durable permission untuk menulis kemudian.

Writer harus menggunakan existing state lock/fence dan melakukan revalidation predecessor/claim tepat sebelum append.

Hal ini mempersempit time-of-check/time-of-use window tanpa menciptakan write-authority primitive baru.

### Capability v1.2.0 tetap dipertahankan

v1.3.2 tetap membawa capability penting dari v1.2.0:

* **Bounded Emergency Authorization (BEA v2)**;
* **Adaptive Context Representation Planning (ACRP)**;
* **C2GRAPH-native Cognitive Emergency Authorization (CEA)**;
* **Progress Liveness & Bounded Self-Audit**;
* strict runtime-state validation;
* handoff telemetry integrity;
* authority non-interference.

Semua mechanism tersebut dapat memengaruhi strategy, diagnosis, containment, representation, atau continuation admission.

Tidak satu pun otomatis memperluas canonical exact-write authority.

---


## Perubahan pada workflow sebelumnya

Distribusi baru memperluas model tiga lapis lama. Fitur baru **tidak** mengganti Goal aktif, CORE rules, `ACTUAL_WRITE_SET`, rollback, atau terminal reconciliation.

### Jalur sebelumnya dan jalur bersyarat sekarang

```text
Sebelum: Goal -> runbook berskop -> exact write plan -> efek terotorisasi -> verifikasi -> terminal/handoff
Sekarang: Goal -> pemilih kapabilitas opsional -> runbook terpilih -> exact write plan lama
         -> efek terotorisasi -> verifikasi lama + obligation assurance terkait
         -> completion gate / blocker / terminal handoff
```

### Perubahan menurut plane eksekusi

| Plane | Yang ditambahkan | Mengganti proses lama? |
|---|---|---|
| Discovery | Pemilihan kapabilitas dan closure dependensi on-demand | Tidak; hanya runbook terpilih |
| Goal / write | Obligation assurance aplikasi dan keputusan kesiapan completion | Tidak; penulisan tetap membutuhkan Goal dan rencana tepat |
| Keamanan | Ledger hasil test terbatas dan rantai failure/retest | Tidak; izin dan scope keamanan tetap terpisah |
| Continuity | Advisory hook compaction opsional dengan validasi terminal | Tidak; handoff/state lama tetap otoritatif |
| QA / resource | Audit terbatas, fixture allowlist, verifikasi snapshot dan restore tanpa overwrite | Tidak; ini gate QA/rilis dan recovery tambahan |

### Jika obligation terblokir

```text
Kapabilitas terpilih -> obligation wajib -> bukti subjek terkini?
  tidak / hilang / unknown -> BLOCKED atau NOT-RUN -> catat blocker -> handoff/suspend
  ya -> verifikasi independen -> gate terkait valid -> logika completion lama
```

Gate ready bukan izin deploy. Hasil model/pemilih advisory tidak menciptakan write authority. Delivery hook harus dibuktikan di host nyata sebelum disebut aktif.

### Panduan alur per fitur

- [Perutean Kapabilitas Universal](C2GRAPH-UNIVERSAL-CAPABILITY-GUIDE-ID.md)
- [Profil Kapabilitas dan Gate Penyelesaian Aplikasi](C2GRAPH-APPLICATION-ASSURANCE-GUIDE-ID.md)
- [Assurance Ledger, Rekonsiliasi, dan Tutup Periode](C2GRAPH-LEDGER-PERIOD-CLOSE-GUIDE-ID.md)
- [Pengujian Keamanan Terbatas dan Rantai Kegagalan](C2GRAPH-SECURITY-VERIFICATION-GUIDE-ID.md)
- [Batas Hook Compaction dan Resume](C2GRAPH-HOOKS-RESUME-GUIDE-ID.md)
- [Pembatasan Resource dan Workspace Sementara](C2GRAPH-RESOURCE-BOUNDARIES-GUIDE-ID.md)
- [Snapshot, Verifikasi dan Restore Berbasis Allowlist](C2GRAPH-SNAPSHOT-BACKUP-RESTORE-GUIDE-ID.md)
- [Audit Otomatis Terbatas dan Gate Rilis](C2GRAPH-AUDIT-OPERATIONS-GUIDE-ID.md)
- [Migrasi Versi Terjaga dan Continuity Workflow](C2GRAPH-MIGRATION-UPGRADE-GUIDE-ID.md)

### Bagaimana penambahan dipanggil secara nyata

`AGENTS.md` merutekan permintaan relevan ke runbook terpilih. Planner universal adalah command read-only eksplisit, sedangkan `scripts/assurance/application.py` harus dipanggil dengan profil proyek dan obligation untuk menilai kesiapan aplikasi. `scripts/verify/all.py` menjalankan verifier/selftest framework, tetapi tidak dengan sendirinya menguji aplikasi adopter baru atau memanggil completion gate aplikasi nyata. Delivery hook juga bergantung konfigurasi host dan event teramati.

### Yang tidak berubah

- Tiga entry mode (`ADOPT_WITH_GOAL`, `WORKFLOW_ONLY`, `BOOTSTRAP`) dan prioritas Core tetap berlaku.
- Run yang terminal tetap terminal meskipun HANDOFF/CURSOR lama menyebut `open`.
- Approval, basis rollback, fresh pre-state dan exact write scope tetap gate yang berbeda.
- Pengiriman hook native, E2E adopter nyata dan replay patch aset publik resmi tidak dibuktikan melalui panduan ini.
- Modul industri yang tidak dipilih tidak otomatis masuk ke proyek.

### Alur ilustratif: callback pembayaran setelah periode ditutup

1. Profil proyek memastikan `payments`, `webhooks` dan `accounting` relevan; pemilik kebijakan dan yurisdiksi ditentukan.
2. Router memilih runbook terkait, sedangkan gate aplikasi membuat obligation idempotensi, tanda tangan, ledger dan period-lock.
3. Callback provider diuji secara sah: duplikasi pesan tidak boleh menciptakan efek bisnis kedua.
4. Kebijakan tutup periode menentukan reject, adjustment atau reversal; agent tidak boleh mengarang keputusan tersebut.
5. Jika kebijakan/izin/bukti belum lengkap, obligation tetap blocked. UI snapshot yang lolos saja tidak membuktikan Goal selesai.
6. Jika perbaikan disetujui, exact-write, rollback, fresh pre-state dan verifikasi lama tetap berlaku.


# Bagaimana C2GRAPH disusun

## Three-layer context architecture

C2GRAPH tidak menaruh semua rule dan procedure ke dalam satu permanently loaded instruction file.

```text
Layer 1
AGENTS.md
Core invariants + routing
        │
        ▼
Layer 2
Domain scopes
Loaded ketika lane terkait aktif
        │
        ▼
Layer 3
Runbooks
Loaded ketika concrete procedure dijalankan
```

Stable invariant tetap stabil.

Detailed procedure hanya dimuat saat relevan.

Arsitektur ini cache-friendly, tetapi bukan jaminan provider tertentu akan menghasilkan cache hit.

Current control-plane identity:

* 40 contiguous CORE rules;
* 46 routing rows;
* 10 non-template scopes;
* 41 runbooks termasuk template;
* 3 entry prompts;
* 12 staged/continuity prompts.

---

# Entry modes

C2GRAPH mendukung tiga kondisi repository.

| Mode | Digunakan ketika | Product mutation |
| --- | --- | --- |
| `ADOPT_WITH_GOAL` | Existing repository yang akan masuk ke implementation | Hanya setelah Goal admission dan exact write authority |
| `WORKFLOW_ONLY` | Install, repair, atau verify governance C2GRAPH tanpa mengubah product behavior | Tidak |
| `BOOTSTRAP` | Repository baru atau kosong | Gated setelah bootstrap planning |

Missing Goal tidak otomatis error.

`WORKFLOW_ONLY` sengaja mendukung governance work tanpa active Goal.

Entry mode menentukan allowed operational path sebelum implementation dimulai.

---

# Capability utama

## Goal Contract

Goal adalah durable execution state, bukan free-form prompt.

Goal readiness dapat memuat:

* requirements;
* acceptance criteria;
* ambiguity;
* conflict;
* stage ordering;
* human decision;
* guard metrics;
* rollback unit;
* resource/complexity budget;
* forbidden action;
* finish condition;
* lifecycle dan supersession.

Admitted Goal mendefinisikan apa yang ingin dicapai.

Goal tidak memberikan arbitrary write authority.

---

## Exact Write Authority

Sebelum canonical mutation, C2GRAPH melakukan convergence pada exact:

```text
ACTUAL_WRITE_SET
├── CREATE
├── MODIFY
└── DELETE
```

No-op candidate dibuang sebelum authorization.

Real mutation tetap harus memenuhi independent conditions:

```text
semantic scope
      ↓
exact action + path
      ↓
rollback authority
      ↓
fresh expected pre-state
      ↓
effect-time revalidation
      ↓
mutation
```

Jika implementation menemukan mutation tambahan, mutation itu tidak otomatis mewarisi authority.

Ia kembali ke planning/admission.

---

## Transaction-specific rollback

Rollback bukan sekadar “buat backup”.

Mutation berbeda membutuhkan recovery basis berbeda.

Contoh:

* CREATE-only rollback;
* independent distribution before-image;
* VCS/external snapshot untuk customized file;
* exact pre-state capture;
* freshness validation sebelum dependent write;
* named rollback unit untuk staged change.

Pertanyaan intinya:

> **Dapatkah mutation exact ini dibalik menggunakan recovery basis valid yang independen dari mutation itu sendiri?**

Rollback capability tidak memberi semantic permission untuk melakukan mutation.

---

## Mutation-free GATE 0

Repository discovery dimulai secara konservatif.

C2GRAPH membedakan static discovery dari active execution.

Presence dari executable/tool bukan bukti bahwa menjalankannya side-effect free.

Command seperti:

```text
--help
--version
```

tidak otomatis aman untuk semua project tooling.

Package manager, runner, hook, generator, network action, dan project-specific command tetap governed sampai justified.

---

## Secret-aware discovery

Repository scanning tidak seharusnya memasukkan secret ke model context tanpa kebutuhan.

C2GRAPH memisahkan:

* secret-prone file detection;
* structural discovery;
* bounded/redacted inspection;
* accidental exposure;
* incident handling;
* continuation decision.

Tujuannya adalah memahami repository dengan meminimalkan unnecessary secret-value admission.

---

# Bounded Evidence Read v2

Reading juga merupakan governed operation.

Bounded Evidence Read v2 membatasi physical evidence admission melalui:

* physical line count;
* normalized evidence bytes;
* per-line character size;
* path containment;
* generation-bound continuation;
* opaque cursor;
* stale-cursor detection;
* bounded JSONL handling;
* bounded archive inspection;
* Unicode-safe handling;
* freshness;
* strategy review setelah repeated blind pagination.

```text
large artifact
      │
      ▼
bounded evidence window
      │
      ├── evidence cukup? ──► lanjut workflow
      │
      └── belum cukup?
              │
              ▼
        cursor / targeted search
        range / parser / tail
        atau strategy change
```

Bounded read bukan endless bounded scrolling.

Jika sequential pagination tidak lagi efektif, workflow harus mengganti retrieval strategy.

Successful read juga tidak bypass Goal, freshness, evidence, atau write-authority gates.

---

# Partition-first discovery

Repository-wide search dapat berbahaya dalam monorepo besar.

Dua aplikasi dapat memiliki nama class, route, DTO, config, database concept, atau directory structure yang sama.

Match yang technically correct dari aplikasi yang salah tetap merupakan evidence yang salah.

C2GRAPH karena itu mendukung explicit corpus/application partition.

Fallback conceptual ladder:

```text
Graph
  ↓
Partitioned Search
  ↓
Direct Bounded Evidence Read
```

Jika search lebih baik daripada graph untuk exact question, search boleh dipakai.

Graph adalah retrieval mechanism.

Graph bukan ideology dan bukan authority.

---

# Per-application index boundaries

Satu repository-wide index tidak otomatis desirable.

Untuk multi-application repo, C2GRAPH dapat membatasi index ke application atau justified partition agar cross-product contamination berkurang.

Index bersifat derived dan rebuildable.

Index tidak menjadi execution authority.

---

# Typed Graph Plane

C2GRAPH memisahkan **work lineage** dari **domain knowledge**.

### Work lineage

Merepresentasikan relationship di antara:

* Objective;
* Goal;
* Task;
* Run;
* Artifact;
* Commit;
* Evaluation;
* Metric;
* Checkpoint.

### Domain knowledge

Merepresentasikan:

* Source;
* Claim;
* relation;
* contradiction;
* evidence;
* decision;
* lesson.

Operational concerns juga dipisahkan menjadi:

```text
Control
Execution
Artifact
Graph
Evaluation
```

Tujuannya adalah mencegah satu conversation transcript berubah menjadi workflow engine, database, memory store, authority ledger, dan audit log sekaligus.

Current graph schema memiliki 20 node kinds dan 22 edge kinds.

---

# Progressive query architecture

C2GRAPH tidak mewajibkan graph database.

Query layer berkembang hanya jika complexity benar-benar membutuhkannya:

```text
Repository JSONL
      ↓
jq / bounded filters
      ↓
SQLite / DuckDB
      ↓
Graph database
hanya jika deeper traversal
memang membenarkan complexity
```

Higher-level index adalah derived infrastructure.

Ia tidak menjadi source of execution authority.

---

# Failure Memory

Failure seharusnya bertahan lebih lama daripada conversation tempat failure itu terjadi.

Failure state dapat menyimpan:

* apa yang gagal;
* failure class;
* blocked action;
* observed signature;
* known cause;
* wrong approach;
* `correct_approach`;
* evidence;
* retry policy;
* lifecycle.

Sebelum mengulang relevant open failure, workflow dapat bertanya:

> **Apa yang secara material berubah sejak approach ini gagal terakhir kali?**

Hal ini mengurangi failure loop yang hanya terjadi karena session amnesia.

---

# Durable Lessons

Resolved failure dan human correction dapat menjadi reusable lesson.

Lesson mempertahankan provenance dan ageing semantics.

Dengan demikian repository dapat membedakan:

* current evidence;
* confirmed lesson;
* stale knowledge;
* unconfirmed assumption.

Ini adalah repository-level operational learning.

Bukan klaim bahwa foundation model melakukan self-retraining.

---

# Workflow discipline

Membaca rulebook bukan bukti rulebook tersebut dijalankan.

C2GRAPH membuat discipline observable melalui:

* preflight state;
* per-turn status;
* sequencing rule;
* violation class;
* violation ledger;
* escalation;
* session-start recall;
* tooling-gap classification.

Observable failure dapat termasuk:

* melewati required phase;
* claim tanpa evidence;
* terus bekerja setelah STOP;
* scope creep;
* menebak human answer;
* resume tanpa rehydration;
* mengulang work yang masih valid;
* retry known failure tanpa material change;
* menggunakan activity sebagai substitusi semantic progress.

Repeated procedural violation dapat menghentikan run.

---

# Progress Liveness & Bounded Self-Audit

Visible activity tidak selalu semantic progress.

C2GRAPH dapat membedakan:

* semantic progress;
* benign external wait;
* legitimate long-running work;
* accounting inconsistency;
* stagnation.

Liveness finding bersifat diagnostic.

Ia tidak dapat membuat `ACTUAL_WRITE_SET`, bypass Goal authority, atau mengubah diagnostic classification menjadi mutation permission.

Stagnation recovery masuk ke CORE-40 Workflow Convergence agar self-audit tidak berputar tanpa batas.

---

# Workflow Convergence

Progress detection dan recovery termination adalah responsibility berbeda.

C2GRAPH menggunakan semantic frontier yang mencakup goal revision, task, next action, evidence, decision, actual write set, blocker, gate state, dan completion state.

Repeated activity terhadap frontier yang sama tidak dihitung sebagai progress.

```text
unchanged frontier
        │
        ▼
NO_PROGRESS_OBSERVED
        │
        ▼
DIAGNOSE_ONCE
        │
        ▼
masih unchanged
        │
        ▼
STOP_BUSY_LIVELOCK
```

Closed invalidator dapat mereset convergence budget secara sah.

External wait tetap evidence-bound.

Completion tetap sticky.

Verification reuse tetap PASS-only dan exact-identity bound.

Workflow Convergence menentukan apakah recovery boleh continue, wait, replan, close, atau stop.

Workflow Convergence tidak menciptakan repository-write authority.

---

# Terminal Reconciliation dan successor admission

Convergence menjawab apakah reasoning/recovery harus berlanjut.

Terminal reconciliation menjawab apakah durable continuity boleh memperlakukan run sebagai resumable.

Same-run resume:

```text
CURSOR / HANDOFF
        │
        ▼
exact run_id
        │
        ▼
AgentRun plane
        │
        ├── ACTIVE / resumable ──► normal validation
        │
        └── COMPLETED
                │
                ▼
        same-run resume rejected
```

Jika `COMPLETED` AgentRun memiliki `evaluation_id`, exactly one matching same-run evaluation dengan verdict `kept` dibutuhkan untuk independently verified completion.

Successor baru membutuhkan lebih dari identifier baru:

```text
verified terminal predecessor
        │
        ▼
terminal_successor
        │
        ├── predecessor_run_id
        ├── predecessor_evaluation_id jika required
        ├── closed successor class
        ├── meaningful semantic_delta
        ├── basis_ref
        ├── basis_sha256
        └── basis_claim_id
                │
                ▼
claim belum pernah consumed?
        │
        ▼
lock / fence + immediate revalidation
        │
        ▼
append successor handoff state
```

Fail-closed property utama:

* stale `open` cursor tidak dapat reactivate completed run;
* explicit non-`kept` evaluation tidak dapat dikalahkan oleh weaker completion evidence;
* duplicate/malformed run/evaluation planes fail closed;
* predecessor evaluation binding harus exact ketika ada;
* `basis_claim_id` harus canonical dan globally unique dalam handoff ledger;
* token-prefix trick seperti `CLAIM-1` terhadap `CLAIM-10` ditolak;
* proposal reshaping tidak dapat menggunakan ulang consumed semantic claim;
* copied evidence tidak menciptakan claim baru;
* direct/symlink-aliased control-plane evidence ditolak;
* admission tetap read-only dan tidak memperluas write authority.

---

# Ratchet loops

C2GRAPH mendukung bounded iterative optimization hanya ketika result dapat diukur.

```text
baseline
   ↓
change
   ↓
measure
   ↓
target improved AND guard preserved?
   ├── yes ──► keep + evidence
   └── no  ──► revert
```

Ratchet bukan “terus mencoba sampai terlihat lebih baik”.

Iteration tetap bounded, measurable, dan reversible.

---

# Blocker convergence

Tidak semua problem adalah terminal blocker.

C2GRAPH membedakan condition yang:

* memungkinkan safe continuation;
* membutuhkan narrowed claim;
* membutuhkan replanning;
* membutuhkan human decision;
* atau benar-benar membutuhkan STOP.

True gate-red condition fail closed.

Recoverable tooling issue tidak perlu menghentikan unrelated work.

---

# Handoff, checkpoint, dan resume

Long-running work akhirnya melewati:

* context compaction;
* tool budget;
* rate limit;
* process restart;
* model/session boundary;
* human pause.

C2GRAPH memperlakukan continuity sebagai explicit state, bukan hanya conversation memory.

Handoff dapat mempertahankan:

* active objective;
* Goal;
* current stage;
* completed work;
* remaining work;
* evidence;
* decision;
* blocker;
* changed paths;
* rollback state;
* next action;
* continuity lineage.

Resumed session harus rehydrate dan memeriksa divergence.

Ia tidak berpura-pura memiliki perfect memory dari session sebelumnya.

Jika repository write tidak authorized, continuity dapat tetap berada di luar target bytes.

Checkpoint tidak boleh membuat permission untuk menulis dirinya sendiri.

v1.3.2 menambahkan terminal reconciliation agar continuity juga tidak dapat menghidupkan kembali run terminal.

---

# Resource epochs

Resource exhaustion tidak sama dengan logical completion.

Run dapat menjadi:

```text
ACTIVE
   ↓
SUSPENDED / RESUMABLE
   ↓
new resource epoch
   ↓
continue exact remaining work
```

Completed discovery atau phase tidak seharusnya restart hanya karena host session berubah.

---

# Tool governance

C2GRAPH tidak menganggap documentation selalu mencerminkan real tool use.

Tool discovery dapat membaca actual call sites seperti:

* scripts;
* CI;
* package configuration;
* editor/agent configuration;
* MCP configuration;
* repository configuration;
* observed invocation surfaces.

Hasil dapat dibedakan menjadi:

* tool benar-benar digunakan;
* installed tetapi tidak digunakan;
* conflicting invocation;
* destructive call;
* ambiguous call;
* low-confidence observation.

Confirmed observation dapat menjadi basis generated runbook.

Ambiguity tetap ambiguity, bukan invented procedure.

---

# Runtime-scoped model adapter

C2GRAPH v1.3.2 tetap membawa **C2ModelAdapter v0.5.5** sebagai host-side compatibility boundary.

Adapter menangani perbedaan seperti:

* native tool-call IDs;
* structured arguments;
* JSON-Schema validation;
* streaming assembly;
* runtime-specific parsing;
* reasoning/provider state;
* capability admission;
* unsupported-combination handling;
* effective-route binding;
* provider-scoped cache state.

Unknown model/runtime combination tidak otomatis dipercaya hanya karena namanya mirip supported model.

Unsupported atau unverified combination dapat fail closed.

### Current verified model/provider profiles

C2ModelAdapter v0.5.5 memisahkan public API-facing model identity dari served revision ketika provider memperbarui backend tanpa mengganti public model string.

Current repo contracts mencakup antara lain:

* **DeepSeek V4 Pro** melalui `deepseek-v4-pro`;
* **DeepSeek V4 Flash** melalui `deepseek-v4-flash`;
* **OpenAI GPT-5.6 Luna** termasuk bounded router aliases;
* **OpenAI GPT-5.6 Sol / Terra** pada verified provider/API surface;
* **xAI Grok 4.5** pada current verified xAI profile;
* **xAI Grok 4.6** sebagai watch/quarantine sampai contract resmi yang diperlukan diverifikasi.

Untuk heterogeneous router combo, logical combo name adalah provenance, bukan effective provider authority.

Ketika route berpindah provider, provider-owned reasoning state, response IDs, continuation handles, dan cache state tidak direplay secara buta ke provider berbeda.

---

# Cache-aware adapter

Prompt caching adalah optimization.

Cache bukan authority.

C2GRAPH mempertahankan pemisahan:

```text
cache hit  ≠ correctness
cache hit  ≠ evidence
cache hit  ≠ freshness
cache hit  ≠ write authority
cache hit  ≠ completion
```

Cache behavior tetap provider/runtime scoped.

Cache miss harus tetap merupakan correct execution path.

Tujuannya adalah deterministic cache eligibility dan mengurangi accidental cache churn jika provider mendukungnya.

Tidak ada universal cache-hit percentage yang dijamin.

---

# Tool-call compatibility dan validation

Tool call bukan satu universal wire format.

Model/runtime/provider dapat berbeda pada:

* argument representation;
* tool identifier;
* native call ID;
* parallel call;
* streaming event;
* reasoning/provider state;
* response assembly.

C2GRAPH memisahkan pertanyaan berikut:

```text
Can the model express the call?
             ↓
Can the adapter parse it?
             ↓
Does it satisfy schema?
             ↓
Is required runtime state valid?
             ↓
Is invocation safe?
             ↓
Is execution authorized?
             ↓
Was it semantically the correct tool?
```

Structural validity bukan semantic correctness.

Protocol correctness bukan repository authority.

---

# Requirements provenance

Requirement dapat menyimpan asalnya.

Contoh:

* extracted;
* inferred;
* ambiguous;
* test-derived;
* document-derived.

Hal ini mencegah:

> “agent menginfer ini”

berubah diam-diam menjadi:

> “repository mewajibkan ini”.

Consequential ambiguity tetap human decision boundary.

---

# Tests, documentation, dan evidence

Executable test dan prose memiliki evidence role berbeda.

Test tidak otomatis sempurna.

Documentation juga tidak otomatis authoritative.

Ketika executable behavior dan prose bertentangan, contradiction harus diekspos.

C2GRAPH juga membedakan ordinary:

```text
PASS
```

dari kondisi yang technically pass karena corpus yang diperiksa kosong.

Kondisi seperti itu dilaporkan sebagai vacuous observation, bukan inflated behavioral proof.

---

# Provenance

C2GRAPH dapat merekam provenance seputar:

* model;
* provider;
* source;
* run;
* Goal;
* phase;
* artifact;
* decision;
* evidence;
* human acceptance;
* digest lineage.

Provenance ledger dapat diekspor ke **W3C PROV-O JSON-LD**.

Digest chaining memberikan integrity evidence.

Ia bukan cryptographic authorship proof.

---

# Human decision boundaries

Tidak semua engineering decision dapat atau seharusnya diotomatisasi.

C2GRAPH mempertahankan human gate ketika semantics tidak dapat ditetapkan secara mekanis.

Prinsipnya:

> **Automation stops where evidence stops.**

---

# Single-agent dan multi-agent operation

C2GRAPH tidak menganggap semakin banyak agent otomatis menghasilkan hasil lebih baik.

Sebelum fan-out, workflow dapat menilai:

* apakah success independently verifiable;
* apakah work item benar-benar separable;
* apakah write ownership jelas;
* apakah alternative hypothesis perlu tetap independent;
* apakah persistent state diperlukan;
* apakah concurrency worth coordination cost.

Parallel work menggunakan bounded concurrency dan explicit reduction.

Fan-out tidak berarti unrestricted concurrent writers.

---

# Mobile-specific governance

C2GRAPH memiliki Android/iOS workflow concern untuk:

* build/release;
* device QA;
* release matrix;
* store submission;
* rollback planning.

Mobile release diperlakukan khusus karena distributed binary tidak dapat ditarik kembali seperti server deployment yang dapat diganti secara langsung.

---

# Centralized configuration

Operational threshold berada di:

```text
.agent/config.yml
```

bukan disalin ke banyak unrelated prose surface.

Current generated inventory memiliki:

* **278 configuration keys**;
* **184 digit-aware numeric thresholds**.

Configuration drift dapat diaudit.

---

# Compliance evidence mapping

C2GRAPH menyediakan evidence mapping terkait control structure dari area seperti:

* SOC 2;
* PCI DSS 4.0;
* EU AI Act.

Ini adalah evidence mapping.

Ini bukan certification.

Menggunakan C2GRAPH tidak otomatis membuat system compliant.

---

# Evidence-first reporting

C2GRAPH membatasi claim pada evidence yang benar-benar mendukungnya.

Contoh:

```text
file count              ≠ token saving
cache telemetry         ≠ reasoning quality
validator PASS          ≠ universal correctness
graph citation          ≠ semantic truth
synthetic simulation    ≠ production behavior
protocol correctness    ≠ better model reasoning
```

Tujuannya bukan menghapus uncertainty dari reporting.

Tujuannya adalah mengekspos boundary uncertainty tersebut.

---

# Memulai C2GRAPH

### Existing repository + implementation

Gunakan entry mode `ADOPT_WITH_GOAL` melalui `SCAN-AND-ADOPT.prompt.md`.

### Existing repository + governance only

Gunakan `WORKFLOW_ONLY` melalui `INSTALL-WORKFLOW-ONLY.prompt.md`.

### New atau empty repository

Gunakan `BOOTSTRAP` melalui `BOOTSTRAP-NEW-REPO.prompt.md`.

### Penting

Operational authority tetap berada di shipped:

* executable prompts;
* `AGENTS.md`;
* `.agent/config.yml`;
* routed scopes;
* runbooks;
* schemas;
* verification scripts.

README menjelaskan system.

README tidak override executable contract tersebut.

---

# Dokumentasi

## Panduan fitur baru v1.4.19

[Panduan Fitur Baru](C2GRAPH-V1.4.19-FEATURE-GUIDES-ID.md). Panduan dan katalog publik lama tetap tersedia sebagai referensi rilis sebelumnya. Jangan menyertakan `PUBLIC-AUDIT*`, `PUBLIC-ASSURANCE-CANDIDATE.md`, log atau bukti pengujian dalam ZIP panduan publik.


C2GRAPH v1.3.2 membawa bilingual public-documentation layer.

Public docs menjelaskan current release tetapi tidak menjadi authority plane kedua.

### Mulai dari sini

| Kebutuhan | English | Bahasa Indonesia |
| --- | --- | --- |
| Navigasi seluruh public docs | [Public Docs Index](C2GRAPH-PUBLIC-DOCS-INDEX.md) | [Indeks Dokumentasi Publik](C2GRAPH-PUBLIC-DOCS-INDEX-ID.md) |
| Memahami perubahan v1.3.2 | [Previous Release Overview](C2GRAPH-V1.3.2-RELEASE-OVERVIEW.md) | [Ringkasan Rilis](C2GRAPH-V1.3.2-RELEASE-OVERVIEW-ID.md) |
| Memahami Workflow Convergence | [Workflow Convergence](C2GRAPH-WORKFLOW-CONVERGENCE-GUIDE.md) | [Workflow Convergence ID](C2GRAPH-WORKFLOW-CONVERGENCE-GUIDE-ID.md) |
| Memahami terminal boundary | [Terminal Reconciliation](C2GRAPH-TERMINAL-RECONCILIATION-GUIDE.md) | [Terminal Reconciliation ID](C2GRAPH-TERMINAL-RECONCILIATION-GUIDE-ID.md) |
| Memeriksa exact distribution path | [File Map](C2GRAPH-FILE-MAP.md) | [File Map ID](C2GRAPH-FILE-MAP-ID.md) |

### Catalog arsitektur dan source

| Area | English | Bahasa Indonesia |
| --- | --- | --- |
| CORE rules, routes, control-plane inventory | [Control Plane Catalog](C2GRAPH-CONTROL-PLANE-CATALOG.md) | [Katalog Control Plane](C2GRAPH-CONTROL-PLANE-CATALOG-ID.md) |
| Seluruh shipped runbook | [Runbook Catalog](C2GRAPH-RUNBOOK-CATALOG.md) | [Katalog Runbook](C2GRAPH-RUNBOOK-CATALOG-ID.md) |
| Script inventory dan verification topology | [Script & Verification Catalog](C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md) | [Katalog Script & Verification](C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md) |

### Current measured repository identity

Angka ini diukur dari final v1.3.2 source tree, bukan diwarisi dari dokumentasi release lama.

| Surface | English | Indonesia |
| --- | ---: | ---: |
| Physical files | **248** | **248** |
| Distribution-manifest entries | **239** | **239** |
| Python scripts | **72** | **72** |
| Validator/gate files | **41** | **41** |
| Registered checks | **43** | **43** |
| CORE rules | **40** | **40** |
| Routing rows | **46** | **46** |
| Runbooks termasuk template | **41** | **41** |
| Graph-engineering documents | **28** | **28** |
| Configuration keys | **278** | **278** |
| Node kinds | **20** | **20** |
| Edge kinds | **22** | **22** |

`CORE-40` adalah Workflow Convergence dan non-authoritative terhadap repository write.

### Public-documentation coverage boundary

Public bundle terdiri dari **18 Markdown files**: README pair dan bilingual Release Overview, Workflow Convergence, Terminal Reconciliation, File Map, Runbook Catalog, Script/Verification Catalog, Control-Plane Catalog, serta Public Docs Index.

Public layer adalah explanatory/navigation surface.

Executable source distribution jauh lebih besar dan memiliki internal graph-engineering docs, schemas, runbooks, prompts, scripts, adapter files, serta governance artifacts yang direpresentasikan melalui catalogs dan tidak perlu diduplikasi menjadi prose publik.

---

# Riwayat verification snapshot (v1.3.2)

C2GRAPH v1.3.2 menggunakan bounded current-release evidence.

Historical v1.2.0 results tidak diam-diam diganti label menjadi v1.3.2 evidence.

Untuk English dan Indonesian edition:

| Surface | Result |
| --- | ---: |
| Registered checks | **43/43 PASS** |
| Failed registered checks | **0** |
| Vacuous pristine-template observations | **4 per edition** |
| Populated aggregate selftest | **0 violations** |
| Terminal directed/contract cases | **39/39 PASS per edition** |
| Built-in finite terminal matrix | **42 states, 0 mismatches per edition** |
| Built-in deterministic successor mutations | **20,000, 0 mismatches per edition** |
| Independent terminal truth matrix | **24 states, 0 mismatches per edition** |
| Independent successor mutation oracle | **100,000, 0 mismatches per edition** |
| Claim-identity chains | **10,000 chains, 0 violations per edition** |
| Real handoff-ledger reuse chains | **250 chains, 0 reuse violations per edition** |
| Numbered flowcharts | **72** |
| Flowchart structural QA | **PASS** |
| Public-doc structural audit | **PASS** |

Source verifier diulang dari working tree dan fresh package extraction.

Final packaging acceptance juga memeriksa CRC, duplicate/unsafe path, nested-component parity, dan SHA-256 manifest.

Empat registered checks dapat menjadi vacuous di pristine template karena live corpus sengaja kosong: live HANDOFF, goals log, lessons, dan failure recall.

Populated selftests menguji mechanism tersebut secara terpisah.

README tidak mengubah empty-corpus success menjadi behavioral proof.

### Direct upgrade dari v1.2.0

Karena v1.2.0 adalah previous public release, v1.3.2 menyediakan direct guarded upgrade.

Upgrade diikat secara cryptographic ke official **237-file v1.2.0 source inventory** yang dipertahankan oleh prior guarded-upgrade lineage.

Current delta:

```text
English
CREATE  11
MODIFY  27
DELETE   0

Indonesia
CREATE  11
MODIFY  26
DELETE   0
```

Source-inventory parity, target parity, already-upgraded detection, tamper rejection, payload CRC, duplicate-path check, dan unsafe-path check telah dijalankan.

Complete original v1.2.0 repository bytes tidak tersedia di artifact environment release build.

Karena itu release tidak mengklaim fresh physical:

```text
v1.2.0 bytes
→ apply
→ verify
→ rollback
→ byte-identical v1.2.0
```

cycle.

Boundary tersebut dinyatakan secara eksplisit dan tidak diganti oleh reconstructed baseline palsu.

### Evidence boundary

Verification di atas adalah finite deterministic/process/package evidence.

Ia tidak membuktikan universal bug absence, universal security, arbitrary crash/power-loss safety, distributed consensus safety, correctness terhadap hostile external concurrent writer, live-provider uptime/correctness, guaranteed cache-hit/cost reduction, improved foundation-model reasoning, atau correctness pada setiap future OS/runtime/provider version.

Release claim yang tepat adalah **bounded verification**, bukan universal proof.

---

<details>
<summary><strong>Expanded C2GRAPH v1.3.2 capability map</strong></summary>

Version 1.3.2 mencakup major capability families berikut:

1. Three-layer context architecture
2. Three explicit entry modes
3. Goal Contract
4. Exact Write Plan and rollback authority
5. Mutation-free GATE 0
6. Secret-aware discovery
7. Bounded Evidence Read v2
8. Bounded repository scanners
9. Corpus partitions and retrieval discipline
10. Per-application index boundaries
11. Typed Graph Plane
12. Progressive query architecture
13. Durable Lessons
14. Failure Memory and pre-action recall
15. Ratchet loops
16. Workflow discipline and rulebook self-checking
17. Blocker convergence
18. Context-compaction continuity
19. Resource-epoch resume
20. Checkpoints as graph lineage
21. Tool-usage capture from real call sites
22. Test-to-workflow mapping
23. Requirements provenance
24. Human decision boundaries
25. Single-agent / multi-agent architecture selection
26. Bounded agent dispatch
27. Runtime-scoped model adapter
28. Cache-aware adapter
29. Mobile-specific release governance
30. Provenance and W3C PROV-O export
31. Compliance evidence mapping
32. Centralized configuration
33. Evidence-first reporting
34. Bilingual release surface
35. Bounded Emergency Authorization (BEA v2)
36. Adaptive Context Representation Planning (ACRP)
37. C2GRAPH-native Cognitive Emergency Authorization (CEA)
38. Progress Liveness & Bounded Self-Audit
39. Strict runtime-state numeric validation
40. Cross-plane authority non-interference
41. Handoff event-ledger integrity and authority-gated resume telemetry
42. CORE-40 Workflow Convergence and bounded unchanged-frontier recovery
43. Sticky completion, closed re-plan invalidators, and PASS-only exact-identity verification reuse
44. Cross-plane terminal reconciliation across AgentRun, Evaluation, CURSOR, and HANDOFF
45. Evidence-bound terminal-successor admission with globally consumable semantic claim identity

</details>

---

# Apa yang bukan C2GRAPH

C2GRAPH **bukan**:

* foundation model baru;
* klaim bahwa model menjadi intrinsically smarter;
* universal RAG engine;
* kewajiban menggunakan graph database;
* justification untuk unrestricted agent write access;
* jaminan provider prompt-cache hit;
* bukti API cost selalu lebih rendah;
* semantic wrong-tool-selection solver universal;
* hostile-process filesystem sandbox;
* distributed consensus system;
* pengganti human judgment;
* compliance certification;
* proof bahwa seluruh kemungkinan bug sudah dieliminasi.

C2GRAPH mengatur bagaimana engineering work direpresentasikan dan dieksekusi di sekitar model.

Quality dari model, host, tools, evidence, dan human decision tetap penting.

---

# Project-agnostic by design

C2GRAPH tidak membutuhkan programming language, framework, cloud, database, model vendor, repository topology, team size, atau agent count tertentu.

Governance model dimaksudkan berada di sekitar engineering workflow.

Ia tidak mendefinisikan ulang product stack.

---

# Research

### Current v1.3.2 release paper

**C2GRAPH: Compositional Repository Governance for Long-Horizon AI Coding Agents**

Current English dan Indonesian manuscripts adalah **v1.3.2 Release Paper**, publication-dated **3 September 2026**, authored by **Hafizh Albanna**.

**Paper DOI:** [10.5281/zenodo.22259065](https://doi.org/10.5281/zenodo.22259065)

Release paper membahas complete Version 1 architecture, Workflow Convergence, terminal reconciliation, successor admission, current source identity, dan bounded release evidence.

Paper dan software adalah research object yang terpisah.

### Current v1.3.2 software archive

C2GRAPH v1.3.2 software/application release memiliki:

**Software DOI:** [10.5281/zenodo.22259056](https://doi.org/10.5281/zenodo.22259056)

Software version adalah `1.3.2`, released **3 September 2026**, authored by **Hafizh Albanna**, dan distributed under **MIT License**.

### Previous public release

v1.2.0 yang dirilis **22 Agustus 2026** tetap merupakan immutable historical provenance.

v1.3.1 adalah immediate technical predecessor ke v1.3.2 tetapi bukan previous public baseline yang dipakai dalam release narrative ini.

Historical DOI tetap historical provenance dan bukan canonical v1.3.2 identifier.

### Citation

Machine-readable citation metadata disertakan sebagai `CITATION.cff` pada full release.

Current identity:

```text
Software
C2GRAPH v1.3.2
Hafizh Albanna
3 September 2026
DOI: 10.5281/zenodo.22259056

Preferred research citation
C2GRAPH v1.3.2 Release Paper
Hafizh Albanna
3 September 2026
DOI: 10.5281/zenodo.22259065
```

---

# License

C2GRAPH dirilis menggunakan **MIT License**.

---

# Author

**Hafizh Al-Banna**

LinkedIn:

https://www.linkedin.com/in/hafizh-al-banna-63275410

---

## Prinsip akhir

C2GRAPH dibangun di sekitar satu gagasan:

> **AI agent tidak seharusnya dipercaya hanya karena terdengar confident.**
>
> **Ia hanya dapat dipercaya sejauh objective, evidence, authority, state, provenance, verification, terminal truth, dan rollback-nya dapat diperiksa.**

Itulah **Convert to Graph Engineering**.
