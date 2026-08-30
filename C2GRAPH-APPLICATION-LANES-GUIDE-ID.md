# Panduan Application Lanes C2GRAPH

**English:** [C2GRAPH-APPLICATION-LANES-GUIDE.md](C2GRAPH-APPLICATION-LANES-GUIDE.md)

> Scope: C2GRAPH v1.2.0. Scope file memperketat governance untuk domain tertentu; scope tidak mengganti root authority.

## 1. Mengapa lane diperlukan

Backend API, database migration, mobile release, payment-sensitive work, dan UI change memiliki failure mode berbeda. C2GRAPH memecahnya ke scoped rule surfaces agar agent dapat memuat kontrol relevan tanpa membuat root router tidak terbatas.

## 2. Lane yang diekspos publik

| Scope | Fokus utama |
|---|---|
| `backend-api.md` | endpoint, service, authn/authz, background jobs, integration |
| `data-schema.md` | table, column, index, relation, migration, seed, heavy query |
| `frontend-web.md` | page, component, client routing/state, styling, asset |
| `infra-deploy.md` | environment, secret, DNS, CDN, pipeline, runtime config, deploy |
| `mobile-android.md` | Android source, Gradle, resources, Play metadata |
| `mobile-ios.md` | iOS source, Xcode project/config, asset, entitlement |
| `mobile-native.md` | shared/cross-platform native behavior |
| `payments-sensitive.md` | pekerjaan payment-sensitive dengan standard lebih ketat |
| `qa-tooling.md` | test, QA harness, browser automation, CI/tool integration |
| `ui-ux.md` | typography, color, spacing, icon, naming, copy, visual quality |
| `_TEMPLATE.md` | template untuk scoped lane baru |

## 3. Multi-lane task

Satu task dapat memerlukan banyak scope sekaligus. Memuat beberapa scope tidak berarti write authority menjadi gabungan wildcard. Semantic scope dan `ACTUAL_WRITE_SET` tetap path-specific.

## 4. Mobile governance

Mobile punya konsekuensi distribusi yang berbeda dari web/server: binary yang sudah dikirim ke user tidak dapat “ditarik kembali” seperti mengganti server deployment. Karena itu platform scope harus dibaca bersama runbook build/release, device QA, store submission, dan rollback planning.

## 5. Monorepo/index boundary

Index dan evidence harus ditempatkan pada application/corpus yang tepat. Evidence dari satu app tidak boleh diam-diam menjawab pertanyaan tentang app lain. `monorepo-index-placement` adalah mekanisme operasional yang mendukung boundary ini.

## 6. Sensitive lane

`payments-sensitive` adalah overlay yang lebih ketat. Evidence gap mempersempit atau menghentikan aksi; ambiguity tidak memperbesar scope.
