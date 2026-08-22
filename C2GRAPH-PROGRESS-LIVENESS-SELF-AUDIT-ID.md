# Progress Liveness dan Bounded Self-Audit

C2GRAPH membedakan **activity**, **diagnostic progress**, dan **semantic progress**. Tool call, heartbeat, read, cache hit, output reviewer, dan checkpoint write adalah evidence activity; semuanya tidak otomatis mereset semantic-progress clock.

## Klasifikasi

Threshold default soft/hard tanpa semantic progress adalah 7.200 dan 28.800 detik. Diagnosis dapat dipicu lebih awal oleh repeated failure, ratchet exhaustion, checkpoint stagnation, tooling tidak sehat, route churn persisten, accounting regression, stale goal, red gate, atau declared action deadline yang terlewati. External wait dengan evidence dan long-running action dengan future deadline valid tetap dipisahkan.

## Strict input boundary - 19 Agustus 2026

JSON snapshot/finding divalidasi berdasarkan **tipe dan nilai**. Boolean JSON bukan number/integer; numeric string ditolak pada boundary numerik JSON; integer counter tidak boleh pecahan; angka wajib finite; dan deadline/confidence harus memenuhi range. Parsing threshold konfigurasi tetap boundary terpisah karena konfigurasi YAML/host dapat secara sah berasal dari teks.

Ini menutup `C2G-PL-20260819-01`. Defect teramati dapat memengaruhi correctness klasifikasi/input; tidak ditemukan jalur yang memperluas repository-write authority.

## Bounded audit dan authority

Suspected/hard stagnation dapat membuat maksimal empat worker diagnostic C2GRAPH untuk read/reason/review. Finding bersifat advisory, `canonical_write=false`, harus memakai lane terdaftar, terikat revision/evidence, dan direduksi fail-closed. Refutation lane secara aktif berusaha membuktikan stagnation salah. Repository change tetap memerlukan semantic scope, exact PHASE 3.W `ACTUAL_WRITE_SET`, rollback basis, immediate freshness, dan effect verification. BEA, CEA, ACRP, cache/model state, maupun severity stagnation tidak menjadi inherited write authority.

## Evidence yang dieksekusi

Evidence saat ini adalah **32/32 directed scenario per edition + 65.536/65.536 state** dari declared 16-Boolean abstraction dengan **0 observed invariant violation**. Lane adversarial terpisah menjalankan **40.000 malformed numeric mutation dengan 0 observed escape**. Exhaustiveness hanya berlaku pada abstraksi yang dideklarasikan.

Surface utama: `.agent/progress.schema.md`, `.agent/runbooks/progress-liveness.md`, `scripts/lib/c2progress.py`, `scripts/progress/watch.py`, `scripts/selftest/progress_liveness.py`, dan `scripts/verify/progress_liveness.py`.
