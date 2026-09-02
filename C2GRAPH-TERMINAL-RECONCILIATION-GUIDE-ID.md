# Terminal Reconciliation dan Admission Successor - v1.3.2

## Mode resume

1. Resolve durable cursor dan exact `run_id`.
2. Baca AgentRun plane. Record `COMPLETED` yang unik membuat resume run yang sama terminal walau continuity `open`.
3. Bila `evaluation_id` ada, wajib tepat satu evaluation `kept` untuk run yang sama. Kontradiksi fail-closed.
4. Jalankan `handoff/validate.py --require-resumable` dan `handoff/terminal_reconcile.py` mode default.

## Mode successor

Run berbeda bukan progress dengan sendirinya. `--successor-json FILE` memerlukan predecessor terverifikasi independen
dan `terminal_successor` dengan binding predecessor, class/delta closed-list, `basis_ref`, `basis_sha256`, serta
`basis_claim_id` canonical yang unik global. Bila evaluation ID ada, `predecessor_evaluation_id` harus exact.

Claim identity dikonsumsi sekali. Proposal reshaping atau menyalin evidence tidak dapat membuat successor berikutnya.
Basis evidence tidak boleh berupa continuity/audit state langsung maupun alias symlink. Helper read-only, jadi caller
wajib memegang state lock yang ada dan revalidasi tepat sebelum append.
