# C2GRAPH v1.4.19 | Panduan proses fitur baru

> Navigasi panduan publik. Tidak memuat bukti audit atau laporan pengujian.

## Hubungan fitur baru dengan workflow lama

```text
Existing entry mode / Goal
     |
Current source + bounded discovery
     |
Optional universal capability selection
     |
Requirements, exact write set, approvals and rollback
     |
Authorized implementation (when applicable)
     |
Existing verification + applicable new assurance obligations
     |
Terminal / handoff / suspend, governed by current state
```

**Yang dipertahankan:** Otoritas Goal, exact write set, rollback, terminal-state truth, dan tidak ada modifikasi source otomatis oleh public docs.

## Panduan

- [Perutean Kapabilitas Universal](C2GRAPH-UNIVERSAL-CAPABILITY-GUIDE-ID.md)
- [Profil Kapabilitas dan Gate Penyelesaian Aplikasi](C2GRAPH-APPLICATION-ASSURANCE-GUIDE-ID.md)
- [Assurance Ledger, Rekonsiliasi, dan Tutup Periode](C2GRAPH-LEDGER-PERIOD-CLOSE-GUIDE-ID.md)
- [Pengujian Keamanan Terbatas dan Rantai Kegagalan](C2GRAPH-SECURITY-VERIFICATION-GUIDE-ID.md)
- [Batas Hook Compaction dan Resume](C2GRAPH-HOOKS-RESUME-GUIDE-ID.md)
- [Pembatasan Resource dan Workspace Sementara](C2GRAPH-RESOURCE-BOUNDARIES-GUIDE-ID.md)
- [Snapshot, Verifikasi dan Restore Berbasis Allowlist](C2GRAPH-SNAPSHOT-BACKUP-RESTORE-GUIDE-ID.md)
- [Audit Otomatis Terbatas dan Gate Rilis](C2GRAPH-AUDIT-OPERATIONS-GUIDE-ID.md)
- [Migrasi Versi Terjaga dan Continuity Workflow](C2GRAPH-MIGRATION-UPGRADE-GUIDE-ID.md)

## Mulai dari mana

1. Baca `README.md` (atau `README-ID.md` untuk C2GRAPH) lalu periksa Goal aktif.
2. Pilih hanya panduan fitur yang sesuai requirement proyek.
3. Ikuti runbook/script kanonis yang disebutkan panduan; periksa gate yang berlaku.
4. Laporkan kondisi unsupported atau unverified apa adanya; tanpa klaim completion universal.
