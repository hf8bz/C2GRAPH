# Ringkasan Rilis C2GRAPH v1.3.2 Terminal Reconciliation

Tanggal rilis: 3 September 2026

C2GRAPH v1.3.2 adalah rilis korektif backward-compatible di atas paket v1.3.1. CORE-40 Workflow Convergence tetap
dipertahankan, dengan boundary terminal lintas-plane agar continuity stale tidak membuka kembali AgentRun
`COMPLETED`. Resume dan admission terminal-successor adalah dua mode executable yang berbeda.

## Boundary terminal baru

- `COMPLETED` mengalahkan CURSOR/HANDOFF `open` yang stale untuk resume run yang sama.
- Bila predecessor membawa `evaluation_id`, tepat satu evaluation `kept` untuk run yang sama diperlukan untuk close
  terverifikasi.
- Evaluation kontradiktif/non-kept fail-closed walaupun `used_as_evidence` bernilai true.
- Successor memerlukan `terminal_successor`, digest evidence exact di luar control plane, dan `basis_claim_id` canonical
  yang unik global serta hadir sebagai exact token dalam evidence UTF-8.
- Claim dikonsumsi sekali sepanjang handoff ledger. Mengubah class, semantic delta, path/copy evidence, model, reviewer,
  sesi, atau run/task ID tidak membuat successor kedua.
- Basis langsung atau alias symlink dari `.agent/state/**`, `.agent/runs/**`, `.agent/graph/**`, atau `backups/**` ditolak.
- Helper admission read-only. Writer tetap membutuhkan state lock/fence yang ada dan wajib revalidasi tepat sebelum
  append checkpoint. Tidak ada repository-write authority baru.

## Identitas source terukur

Per edisi bahasa: 239 file shipped/distribution, 72 skrip Python, 41 validator terkirim, 43 check terdaftar, 40 aturan
CORE kontigu, 46 routing row, 41 runbook termasuk template, dan 28 dokumen graph-engineering.

## Boundary verifikasi

Audit rilis menjalankan ulang full verifier EN dan ID dari fresh package extraction serta menguji matriks state terminal,
mutasi successor, chain claim-reuse, handoff projection, package parity, dan struktur upgrade. Evidence ini bounded
falsification, bukan bukti matematis bahwa defect mustahil pada seluruh host masa depan, filesystem crash, atau writer
external concurrent.

## Identitas publikasi

Rilis v1.2.0 tetap immutable history. C2GRAPH v1.3.2 dipublikasikan dengan DOI software
`10.5281/zenodo.22259056`; paper rilis bilingual v1.3.2 dipublikasikan dengan DOI
`10.5281/zenodo.22259065`.
