# Panduan Workflow Convergence - v1.3.2

CORE-40 membatasi recovery pada satu semantic frontier:

`NO_PROGRESS_OBSERVED -> DIAGNOSE_ONCE -> STOP_BUSY_LIVELOCK`

Invalidator closed-list dapat mereset convergence budget secara sah. External wait tetap evidence-bound, completion
sticky, dan reuse verifikasi tetap PASS-only. v1.3.2 menambahkan gate terminal lintas-plane terpisah: semantic
convergence menentukan apakah recovery harus berlanjut, sedangkan terminal reconciliation menentukan apakah durable
continuity boleh memperlakukan run completed sebagai resumable.

Predecessor terminal tidak pernah di-resume. Successor hanya diterima setelah completion terverifikasi independen dan
satu claim unik-global yang evidence-bound. Mode default `terminal_reconcile.py` adalah resume gate;
`--successor-json FILE` adalah admission successor. Keduanya read-only. State lock/fence yang sudah ada menyediakan
serialisasi saat append.
