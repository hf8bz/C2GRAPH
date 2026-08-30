# C2GRAPH File Map — Companion Bahasa Indonesia

**English:** [C2GRAPH-FILE-MAP.md](C2GRAPH-FILE-MAP.md)

> Companion untuk `C2GRAPH-FILE-MAP.md`. Path/filename bersifat language-neutral dan canonical path inventory tetap harus direkonsiliasi terhadap frozen release tree/manifest. Companion ini menjelaskan keluarga path tanpa mengarang path yang belum terlihat.

## Frozen v1.2.0 inventory snapshot yang dipublikasikan

Public File Map melaporkan:

- 228 distributed files per EN/ID edition;
- 64 Python scripts;
- 37 validators/gates;
- 39 registered checks;
- 45 routing rows;
- 40 runbooks termasuk template / 39 non-template;
- 27 graph-engineering docs;
- CORE-01 sampai CORE-39 contiguous.

Authoritative live count disebut berasal dari `scripts/verify/counts.py --json`.

## Major path families

- root prompts/rules/release files;
- `.agent/config.yml`, routes, baselines, rule registry, distribution manifest, counts;
- Goal/requirement/failure/lesson/provenance/handoff schemas dan state;
- graph schema + graph-engineering docs;
- scope files untuk backend/data/frontend/infra/mobile/payments/QA/UI;
- runbooks;
- root/staged/resume prompts;
- Python scripts untuk scan/read/state/goal/graph/handoff/provenance/adapter/selftest/verify;
- adapter package/modules.

## Important reconciliation issue

Tabel public File Map yang terlihat menyebut 36 Markdown runbook bernama, sedangkan snapshot count mengatakan 40 termasuk template. Ini **bukan bukti otomatis bahwa release archive salah**; ini berarti public inventory belum self-reconciling. Empat nama tidak boleh diciptakan berdasarkan asumsi.

Hal yang sama berlaku untuk 64 Python scripts: exact path list harus dihasilkan dari authoritative release tree/manifest.

## Frozen bundle versus current public docs

Count dokumentasi pada frozen v1.2.0 release tidak boleh diubah hanya karena public repository menerima explanatory docs tambahan setelah release. Historical release identity harus dipertahankan; current docs inventory dicatat terpisah.
