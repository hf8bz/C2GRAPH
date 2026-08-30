# Matriks Keputusan Entry Mode C2GRAPH

**English:** [C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md](C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md)

> Companion Bahasa Indonesia untuk `C2GRAPH-ENTRY-MODE-DECISION-MATRIX.md`.

## Tiga mode formal

### `ADOPT_WITH_GOAL`
Pilih bila repository sudah berisi project dan ada objective implementasi nyata yang dapat mengubah product/repository behavior. Direct entry prompt: `SCAN-AND-ADOPT.prompt.md`.

### `WORKFLOW_ONLY`
Pilih bila repository existing hanya memerlukan install, repair, upgrade, atau verification C2GRAPH control plane. Direct entry prompt: `INSTALL-WORKFLOW-ONLY.prompt.md`. Product behavior dan logical goal state tidak boleh berubah.

### `BOOTSTRAP`
Pilih bila repository kosong atau baru. Direct entry prompt: `BOOTSTRAP-NEW-REPO.prompt.md`.

## Decision table

| Repo | Product behavior harus berubah? | Tujuan hanya C2GRAPH maintenance? | Mode |
|---|---:|---:|---|
| existing/non-empty | ya | tidak | `ADOPT_WITH_GOAL` |
| existing/non-empty | tidak | ya | `WORKFLOW_ONLY` |
| empty/new | sesuai bootstrap objective | tidak relevan | `BOOTSTRAP` |

## Anti-pattern

- Jangan menyebut formal mode sebagai `SCAN`; itu adalah nama keluarga prompt/adoption action.
- Jangan menggunakan workflow-only untuk melakukan product change tersembunyi.
- Jangan membuat Goal palsu hanya karena workflow-only tidak mempunyai active implementation goal.
- Jangan memilih bootstrap untuk existing project hanya karena struktur C2GRAPH belum ada.
- Jangan menganggap entry mode memberi wildcard write authority; seluruh normal mutation contract tetap berlaku.

## Lanjut membaca

- `C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE-ID.md`
- `C2GRAPH-WORKFLOW-ONLY-GUIDE-ID.md`
- `C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE-ID.md`
