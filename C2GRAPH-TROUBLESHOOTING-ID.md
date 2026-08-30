# Troubleshooting C2GRAPH — Bahasa Indonesia

**English:** [C2GRAPH-TROUBLESHOOTING.md](C2GRAPH-TROUBLESHOOTING.md)

> Diagnosis untuk run yang berhenti, menolak aksi, tidak dapat resume, atau menghasilkan state/verification inconsistency.

## 1. Mulai dari klasifikasi, bukan retry

Periksa secara berurutan:

1. repository root/boundary benar?
2. formal entry mode benar?
3. Goal/objective state sesuai mode?
4. route/scope/runbook yang berlaku sudah dipilih?
5. evidence/freshness cukup?
6. exact write/rollback authority ada?
7. secret/path safety memblokir read/write?
8. budget/context/resource epoch bermasalah?
9. model adapter/tool capability valid?
10. verifier gagal karena defect nyata, stale/generated state, atau inventory drift?

## 2. Typical symptoms

### `NOT_FOUND` / `AMBIGUOUS_PATH`
Jangan menganggap empty. Gunakan bounded repair/discovery dan pastikan uniqueness.

### `STALE_CURSOR` / `PROFILE_MISMATCH`
Source generation atau read profile berubah. Ambil evidence baru; jangan teruskan cursor lama.

### Workflow-only ingin product change
Classification salah. Bentuk implementation objective dan gunakan goal-bound path.

### Write refused
Periksa semantic scope, exact `ACTUAL_WRITE_SET`, rollback, fresh pre-state, secret boundaries, dan effect-time authority. Refusal bukan otomatis bug.

### Repeated failure
Baca Failure Memory/pre-action recall dan ubah hypothesis/inputs sebelum retry.

### Resume tidak konsisten
Validasi capsule/checkpoint/event ledger. Jangan mengestimasi exact measurement yang unavailable.

### Verification count berbeda
Regenerate measurement dari source owner. Jangan edit angka dokumentasi agar “match”.

## 3. Escalation

Jika consequential ambiguity tetap tidak bisa dibuktikan secara mekanis, human decision boundary adalah hasil yang benar. Automation berhenti di batas evidence.
