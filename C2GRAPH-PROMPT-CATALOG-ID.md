# Katalog Prompt C2GRAPH

**English:** [C2GRAPH-PROMPT-CATALOG.md](C2GRAPH-PROMPT-CATALOG.md)

> Katalog publik untuk executable/staged/resume prompt C2GRAPH v1.2.0. Nama file dan token mode tidak diterjemahkan.

## 1. Root entry prompts

| Prompt | Formal mode / fungsi |
|---|---|
| `SCAN-AND-ADOPT.prompt.md` | direct execution = `ADOPT_WITH_GOAL` untuk existing repo dengan objective implementasi |
| `INSTALL-WORKFLOW-ONLY.prompt.md` | `WORKFLOW_ONLY` untuk install/repair/upgrade/verify control plane tanpa product change |
| `BOOTSTRAP-NEW-REPO.prompt.md` | `BOOTSTRAP` untuk empty/new repo |

## 2. Aturan pemilihan

1. Nama `SCAN-AND-ADOPT` tidak membuat mode bernama `SCAN`.
2. Workflow-only boleh mendelegasikan internal step sesuai source contract, tetapi direct user entry tetap wrapper workflow-only.
3. Prompt tidak boleh digunakan untuk melewati semantic scope atau exact-write requirement.
4. Stage prerequisite adalah bagian dari contract; jangan mengubah urutan hanya karena prompt berikutnya tersedia.
5. Resume prompt mengembalikan bounded continuity, bukan authority dari memory.

## 3. Staged prompt family

Public File Map mengekspos root entry prompt dan staged/resume prompt family termasuk tahap `00`–`08` serta `97`–`99`. Exact daftar path harus direkonsiliasi terhadap release tree sebelum disebut exhaustive jika public inventory tidak menampilkan semuanya secara self-contained.

## 4. Prompt versus docs

Dokumentasi menjelaskan prompt; executable prompt adalah surface operasional. Bila prose dan executable contract bertentangan, contradiction harus disurfaced dan source contract diperiksa—bukan memilih teks yang paling nyaman.
