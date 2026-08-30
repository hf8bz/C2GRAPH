# Katalog Script dan Verification C2GRAPH

**English:** [C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md](C2GRAPH-SCRIPT-VERIFICATION-CATALOG.md)

> Public File Map melaporkan 64 Python scripts, 37 validators/gates, dan 39 registered checks. Karena path table yang dipublikasikan belum menjadi self-contained exact 64-script list, katalog ini membedakan path yang terlihat dari measured totals.

## Script family yang terlihat pada File Map

| Family/path | Peran publik |
|---|---|
| `scripts/adapter/capability.py` | validate/digest adapter capability document |
| `scripts/budget/check.py` | resource-epoch classification |
| `scripts/goal/gate.py` | goal integrity/active-goal gate |
| `scripts/graph/validate.py` | graph schema/instance validation |
| `scripts/handoff/capsule.py` | resume capsule validation |
| `scripts/handoff/validate.py` | handoff/checkpoint/canary validation |
| `scripts/install/preflight.py` | rollback/write-plan preflight |
| `scripts/interview/ask.py` | optional question queue |
| `scripts/jsonl/validate.py` | JSONL validation |
| `scripts/lessons/decay.py` | lesson age/reconfirmation reporting |
| `scripts/lib/c2common.py` | shared helpers |
| `scripts/lib/c2read.py` | bounded-read primitives |
| `scripts/provenance/export.py` | W3C PROV-O export |
| `scripts/read/core.py` | canonical bounded text evidence CLI |
| `scripts/scan/census.py` | existing-repo census |
| `scripts/scan/plan.py` | new-repo planning census |
| `scripts/scan/redact.py` | secret-safe structural reader |
| `scripts/scan/tooling.py` | static tool inventory |
| `scripts/selftest/*` | subsystem/regression selftests |
| `scripts/state/*` | state lease/migration |
| `scripts/verify/*` | aggregate + specialized verification checks |

## Verification principle

- `scripts/verify/all.py` adalah aggregate registered-check runner.
- `scripts/verify/counts.py --json` adalah authoritative live measurement yang disebut public File Map untuk count snapshot.
- Published number harus diverifikasi dari source owner dan tidak dipelihara manual secara terpisah.
- PASS berlaku untuk contract yang diuji; ia bukan universal-correctness proof.
- Randomized/fuzz/finite-domain result harus diberi boundary domain/harness yang eksplisit.

## Exact-catalog blocker

Jangan menyebut path list ini sebagai “64/64 exact scripts” sebelum tree/manifest menghasilkan exact path set dan count-nya cocok dengan measurement. Missing path tidak boleh ditebak.
