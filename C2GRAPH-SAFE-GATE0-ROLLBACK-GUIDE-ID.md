# Panduan Safe GATE 0 dan Rollback C2GRAPH

**English:** [C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md](C2GRAPH-SAFE-GATE0-ROLLBACK-GUIDE.md)

> Companion Indonesia. GATE 0 adalah phase aman/mutation-free untuk membentuk pemahaman, scope, evidence, dan rollback readiness sebelum write authority digunakan.

## Tujuan GATE 0

GATE 0 mencegah agent mengubah repository saat fakta dasar masih belum mapan. Aktivitas yang sesuai bersifat read-only/bounded: inventory, routing, evidence collection, goal/requirement inspection, risk classification, dan precondition analysis.

## Mutation-free berarti mutation-free

Jangan menulis “hanya file kecil”, memperbaiki typo, membuat helper, mengubah config, atau memperbarui state canonical sebagai convenience pada GATE 0 kecuali source contract secara eksplisit mengklasifikasikan operasi tersebut di phase lain dengan authority yang sah.

## Rollback readiness

Sebelum mutation phase, rollback basis harus diketahui untuk exact target. Rollback bukan pengganti semantic scope.

## Fresh pre-state

Pre-state yang dipakai untuk effect-time decision harus fresh. Jika target berubah setelah planning, authority/preconditions harus dievaluasi ulang.

## Failure behavior

Jika repository boundary, target identity, semantic scope, rollback basis, atau required evidence belum dapat dibuktikan, hasil yang benar adalah stop/narrow/re-plan, bukan memperluas aksi.
