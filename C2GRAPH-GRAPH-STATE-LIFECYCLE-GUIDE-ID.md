# Panduan Siklus Graph dan Runtime State C2GRAPH

**English:** [C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md](C2GRAPH-GRAPH-STATE-LIFECYCLE-GUIDE.md)

> Scope: C2GRAPH v1.2.0. Panduan ini menghubungkan Goal, requirement, graph entity, failure/lesson, checkpoint/handoff, dan resume. State tidak otomatis menjadi authority.

## 1. Prinsip dasar

C2GRAPH memindahkan state engineering tertentu dari conversation history yang transient ke kontrak repository yang dapat diperiksa. State tersebut dapat menjelaskan apa yang diketahui, gagal, diputuskan, atau harus dilanjutkan—tetapi mutation tetap membutuhkan authority yang tepat.

## 2. Goal lifecycle

Goal menyimpan objective secara durable. Goal dapat memiliki lineage/inheritance tetapi tidak memberi wildcard permission untuk menulis seluruh repository. Workflow-only tidak boleh mengarang atau mengaktifkan goal hanya untuk melewati gate.

## 3. Requirement lifecycle

Requirement mempertahankan provenance. Derived requirement harus dibedakan dari explicit requirement. Contradiction atau ambiguity consequential tetap menuju human boundary.

## 4. Typed Graph Plane

Graph mewakili relasi engineering work melalui node/edge yang dideklarasikan. Public Reference menyatakan vocabulary 20 node types dan 20 edge types. Validator harus menolak type yang tidak dideklarasikan.

Graph adalah representasi hubungan, bukan database wajib dan bukan sumber semantic truth otomatis.

## 5. Failure Memory dan Durable Lessons

Failure memory membantu mencegah retry buta. Lesson menyimpan pembelajaran yang dapat digunakan kembali dengan age/reconfirmation rules. Pre-action recall harus terjadi sebelum mengulang strategi yang relevan.

CEA dapat menahan actionability dari exact recalled object tertentu; raw evidence tidak dihapus.

## 6. Checkpoint, handoff, resume

Checkpoint mencatat lineage work state. Handoff/resume membawa state yang cukup untuk melanjutkan tanpa mengandalkan ingatan percakapan. Current checkpoint row bersifat immutable; resume telemetry dicatat sebagai event terpisah ketika persistence diizinkan.

Tidak tersedianya exact counter harus dinyatakan unavailable, bukan diestimasi seolah exact.

## 7. Context compaction dan resource epoch

Compaction/resource exhaustion memerlukan bounded handoff capacity dan resume capsule. Resume tidak boleh merekonstruksi write authority yang hilang hanya dari summary atau memory.

## 8. State freshness

State lama/cached harus diperlakukan sesuai freshness/invalidation contract. Fresh pre-state untuk mutation tetap terpisah dari fakta bahwa state durable pernah ada.
