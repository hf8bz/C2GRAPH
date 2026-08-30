# Panduan Blocker Convergence C2GRAPH

**English:** [C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md](C2GRAPH-BLOCKER-CONVERGENCE-GUIDE.md)

> Companion Indonesia. Blocker adalah state yang harus diklasifikasikan dan direduksi, bukan alasan untuk retry tanpa batas atau berhenti terlalu dini.

## Prinsip convergence

Ketika progress berhenti:

1. identifikasi exact blocker dan evidence-nya;
2. pisahkan blocker internal, external wait, missing evidence, missing authority, tool failure, resource limit, contradiction, atau semantic ambiguity sesuai runbook;
3. tentukan apakah masih ada bounded work independen yang aman;
4. gunakan recovery ladder yang cocok;
5. setelah retry/recovery, ukur apakah blocker benar-benar berubah;
6. stop/escalate bila convergence condition tidak terpenuhi.

## Retry discipline

Failure Memory dan preflight recall mencegah pengulangan strategi yang sama tanpa new evidence. Retry harus mempunyai perubahan hypothesis/input/state yang dapat diuji.

## Jangan mengubah authority

Blocker severity tidak memperluas write scope. Bahkan ketika production incident atau deadline mendesak, emergency path tetap memerlukan bounded human-granted authority seperti BEA bila applicable.

## Reporting

Blocker report harus menjelaskan apa yang diketahui, apa yang belum diketahui, evidence yang mendukung, recovery yang sudah dicoba, dan next bounded action. “Stuck” tanpa state detail tidak cukup untuk resume.
