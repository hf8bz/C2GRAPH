# Panduan Budget dan Resume C2GRAPH

**English:** [C2GRAPH-BUDGET-RESUME-GUIDE.md](C2GRAPH-BUDGET-RESUME-GUIDE.md)

> Companion Bahasa Indonesia untuk resource/context/rate/host-session suspension dan bounded resume.

## Resource epoch

C2GRAPH memperlakukan context/budget/rate/session exhaustion sebagai state transition yang dapat dipersiapkan. Jangan menghabiskan seluruh kapasitas sebelum menyimpan handoff evidence yang cukup.

## Reserved handoff capacity

Sisakan kapasitas untuk menghasilkan bounded resume capsule/checkpoint ketika continuation diperlukan. Capsule harus membawa objective, completed/pending work, evidence references, blockers, authority-relevant state, dan next step sesuai schema—bukan seluruh transcript.

## Resume

Resume memvalidasi capsule/checkpoint dan freshness. Summary atau model memory tidak boleh digunakan untuk menciptakan write authority yang tidak ada. Jika exact counter/state tidak tersedia, nyatakan unavailable.

## Context compaction

Compaction mengubah conversational representation, bukan canonical repository truth. Durable Goal/evidence/graph/failure/lesson/checkpoint state digunakan agar continuity tidak bergantung pada chat memory semata.

## External wait

Benign wait dibedakan dari stagnation. Progress Liveness dapat membantu diagnosis tetapi tidak boleh membuat write authority.
