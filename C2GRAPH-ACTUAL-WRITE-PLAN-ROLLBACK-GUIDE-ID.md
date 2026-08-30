# Panduan Actual Write Plan dan Rollback C2GRAPH

**English:** [C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md](C2GRAPH-ACTUAL-WRITE-PLAN-ROLLBACK-GUIDE.md)

> Companion Bahasa Indonesia untuk exact-write/rollback contract C2GRAPH v1.2.0.

## Prinsip utama

C2GRAPH memisahkan “mengetahui apa yang mungkin perlu diubah” dari “memiliki authority untuk benar-benar mengubahnya”. Mutation hanya boleh dilakukan pada exact `ACTUAL_WRITE_SET` yang telah dimaterialisasi sesuai semantic scope.

## Sebelum write

Sebelum operasi yang mempunyai side effect, pastikan:

1. objective/Goal relevan dan tidak ambiguous secara consequential;
2. evidence cukup untuk semantic scope;
3. exact target/path/action sudah ditetapkan;
4. rollback basis cocok dengan exact target;
5. fresh pre-state diperoleh sedekat mungkin dengan effect time;
6. secret/sensitive boundaries dihormati;
7. verification setelah write sudah diketahui.

## Actual Write Plan

Plan bukan daftar “mungkin disentuh”. Ia harus membedakan planned reasoning surface dari exact write authority. Adjacent cleanup, refactor, dependency update, atau “sekalian” change tidak otomatis masuk write set.

## Rollback

Rollback harus scope-preserving. Kemampuan untuk mengembalikan perubahan tidak memberi izin untuk membuat perubahan yang sejak awal tidak authorized. Rollback target/basis harus cukup spesifik agar restoration dapat diverifikasi.

## Effect-time authority

Authority dievaluasi pada operasi aktual, bukan hanya saat planning. State yang berubah antara plan dan effect dapat membuat precondition stale dan harus menghasilkan stop/re-plan, bukan meneruskan berdasarkan asumsi lama.

## Verification

Setelah mutation, verifikasi immediate local effect lalu jalankan relevant workflow/test/evidence checks. “File berhasil ditulis” bukan completion proof.

## Emergency case

BEA v2 hanya overlay manusia yang bounded dan terikat pada exact plan/action/target/rollback/run/time. Ia tidak membuat wildcard emergency write set.
