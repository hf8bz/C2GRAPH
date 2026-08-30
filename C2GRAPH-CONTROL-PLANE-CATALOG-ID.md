# Katalog Control Plane C2GRAPH

**English:** [C2GRAPH-CONTROL-PLANE-CATALOG.md](C2GRAPH-CONTROL-PLANE-CATALOG.md)

> Companion Bahasa Indonesia untuk `C2GRAPH-CONTROL-PLANE-CATALOG.md`. Scope: C2GRAPH v1.2.0. Dokumen ini memetakan keluarga control plane; ia **tidak** menciptakan authority baru.

## 1. Urutan authority praktis

Urutan baca yang aman adalah:

1. objective dari user/host dan batas repository;
2. formal entry mode dan executable entry prompt;
3. `AGENTS.md` + CORE invariants;
4. `.agent/routes.yaml` + scope yang relevan;
5. runbook yang berlaku;
6. `.agent/config.yml` untuk project binding dan thresholds;
7. Goal/requirement/evidence state yang tepat;
8. PHASE 3.W exact `ACTUAL_WRITE_SET`, rollback basis, dan fresh pre-state;
9. verification sesaat setelah operasi;
10. completion/evidence gates;
11. handoff/resume state bila pekerjaan berlanjut lintas session.

Dokumen penjelasan di level lebih bawah tidak boleh diam-diam mengalahkan source contract executable.

## 2. Surface governance stabil

| Surface | Fungsi publik | Batas authority |
|---|---|---|
| `AGENTS.md` | root governance router + CORE invariants | normative control-plane surface |
| `.agent/routes.yaml` | routing machine-readable | harus konsisten dengan routing Markdown |
| `.agent/core-baseline.md` | baseline identifier rule yang dikirim | mendeteksi rule loss |
| `.agent/rule-registry.md` | ledger kept/moved/absorbed/rejected | rejection perlu governance eksplisit |
| `.agent/prefix.lock` | digest lock untuk skeleton router | mendeteksi drift struktural |
| `.agent/config.yml` | project binding + tunable thresholds | tidak mengganti semantic scope |
| `.agent/distribution-files.txt` | manifest shipped path | basis count/parity |
| `.agent/counts.generated.json` | measurement snapshot turunan | bukan authority independen |
| `.agent/compliance-map.md` | evidence/control mapping | bukan sertifikasi |

## 3. Planes yang tidak boleh dicampur

C2GRAPH memisahkan paling tidak:

- **objective/goal plane** — apa yang ingin dicapai;
- **evidence/requirement plane** — apa yang diketahui dan dari mana asalnya;
- **graph/work-state plane** — hubungan kerja, failure, lesson, checkpoint;
- **strategy/runtime plane** — routing, model adapter, ACRP, cache, dispatch;
- **authority plane** — apa yang benar-benar boleh dimutasi;
- **assurance plane** — verification, provenance, claims, compliance mapping.

Informasi pada satu plane dapat mempengaruhi strategi atau diagnosis tetapi tidak otomatis memperluas exact-write authority. Ini adalah inti **cross-plane authority non-interference**.

## 4. Human decision boundary

Jika semantic scope atau konsekuensi tidak dapat dibuktikan secara mekanis, automation harus berhenti atau mempersempit aksi. Ambiguitas bukan alasan untuk memperluas mutation set.

## 5. Emergency/adaptive/liveness overlays

BEA, ACRP, CEA, cache/routing state, incident severity, dan Progress Liveness dapat mengubah cara kerja, representasi, diagnosis, atau containment. Semuanya tetap tidak boleh membuat wildcard write authority.

## 6. Dokumen terkait

- `C2GRAPH-REFERENCE-ID.md`
- `C2GRAPH-CONFIGURATION-REFERENCE-ID.md`
- `C2GRAPH-RUNBOOK-CATALOG-ID.md`
- `C2GRAPH-SCHEMA-STATE-CATALOG-ID.md`
- `C2GRAPH-EVIDENCE-PROVENANCE-ASSURANCE-GUIDE-ID.md`
