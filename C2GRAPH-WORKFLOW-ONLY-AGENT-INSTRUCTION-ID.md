# Instruksi Agent — C2GRAPH Workflow-Only

**English:** [C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md](C2GRAPH-WORKFLOW-ONLY-AGENT-INSTRUCTION.md)

> Gunakan companion ini untuk menjelaskan intent kepada manusia/agent dalam Bahasa Indonesia. Executable authority tetap berasal dari `INSTALL-WORKFLOW-ONLY.prompt.md` dan source contract yang dikirim.

## Instruksi inti

Kerjakan repository ini dalam `ENTRY_MODE=WORKFLOW_ONLY`.

- Tujuan hanya install, repair, upgrade, atau verification C2GRAPH governance/control plane.
- Jangan mengubah product/application behavior.
- Jangan membuat, mengaktifkan kembali, atau memodifikasi implementation Goal hanya untuk menjalankan maintenance.
- Pertahankan existing Goal records dan lineage sesuai source contract.
- Batasi semua write ke surface C2GRAPH yang memang diizinkan dan exact write plan yang berlaku.
- Pertahankan rollback basis dan fresh pre-state sebelum mutation.
- Jalankan verification yang diwajibkan dan laporkan gap/unknown secara eksplisit.
- Bila product change diperlukan, berhenti dari workflow-only dan minta/establish real implementation objective melalui mode yang tepat.

## Required final semantics

Successful workflow-only run harus konsisten dengan:

```text
ENTRY_MODE: WORKFLOW_ONLY
GOAL_MUTATIONS: 0
PRODUCT_CHANGES: 0
```

Field final lengkap mengikuti executable wrapper. Companion ini tidak mengganti field/schema yang sebenarnya.
