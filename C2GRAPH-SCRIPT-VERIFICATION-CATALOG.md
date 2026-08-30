# C2GRAPH Script and Verification Catalog

**Bahasa Indonesia:** [C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md](C2GRAPH-SCRIPT-VERIFICATION-CATALOG-ID.md)

> C2GRAPH v1.2.0 public catalog. The public File Map reports 64 Python scripts, 37 validators/gates, and 39 registered checks. The path table currently exposed by that File Map does not itself enumerate an obvious complete 64-Python-script set, so this document separates **publicly enumerated paths** from **measured totals**.

## Publicly enumerated `scripts/**` Python paths

| Path | Publicly described role |
| --- | --- |
| `scripts/adapter/capability.py` | adapter capability-document validator/digest |
| `scripts/budget/check.py` | resource-epoch classifier |
| `scripts/goal/gate.py` | goal integrity/active-goal gate |
| `scripts/graph/validate.py` | graph schema/instance validation |
| `scripts/handoff/capsule.py` | resume capsule validator |
| `scripts/handoff/validate.py` | handoff schema/checkpoint/canary validation |
| `scripts/install/preflight.py` | rollback/write-plan preflight |
| `scripts/interview/ask.py` | optional question queue renderer/collector |
| `scripts/jsonl/validate.py` | JSONL validation |
| `scripts/lessons/decay.py` | lesson reconfirmation/age reporting |
| `scripts/lib/c2common.py` | shared repo/config/count helpers |
| `scripts/lib/c2read.py` | bounded-read primitives |
| `scripts/provenance/export.py` | PROV-O export |
| `scripts/read/core.py` | canonical bounded text evidence CLI |
| `scripts/scan/census.py` | existing-repo census |
| `scripts/scan/plan.py` | new-repo planning census |
| `scripts/scan/redact.py` | secret-safe structural reader |
| `scripts/scan/tooling.py` | static tool inventory |
| `scripts/selftest/model_adapter.py` | adapter admission selftest |
| `scripts/selftest/read.py` | bounded-read adversarial selftest |
| `scripts/selftest/run.py` | aggregate populated/regression selftest |
| `scripts/state/lock.py` | TTL state lease |
| `scripts/state/migrate.py` | state schema migration |
| `scripts/verify/adapter.py` | adapter cross-surface verification |
| `scripts/verify/all.py` | aggregate registered-check runner |
| `scripts/verify/ambiguity.py` | normative wording ambiguity check |
| `scripts/verify/claims.py` | published-claim/count checks |
| `scripts/verify/cli.py` | documented CLI flag checks |
| `scripts/verify/compliance.py` | compliance-map validation |
| `scripts/verify/counts.py` | authoritative source measurement |
| `scripts/verify/discipline.py` | workflow/entry/goal/blocker checks |
| `scripts/verify/enforcement.py` | CORE enforcement ratchet |
| `scripts/verify/excludes.py` | exclude/carveout consistency |
| `scripts/verify/golden.py` | retrieval golden-set validation |
| `scripts/verify/hygiene.py` | text/file hygiene |
| `scripts/verify/index_scope.py` | index placement validation |
| `scripts/verify/links.py` | relative Markdown links |
| `scripts/verify/paths.py` | literal path/runtime-artifact checks |
| `scripts/verify/placeholders.py` | unresolved placeholder checks |
| `scripts/verify/prefix.py` | router prefix-lock validation |
| `scripts/verify/preserve.py` | baseline/registry/router preservation |
| `scripts/verify/provenance.py` | decision ledger/digest validation |
| `scripts/verify/read.py` | cross-surface read contract |
| `scripts/verify/recall.py` | failure-memory recall validation |
| `scripts/verify/routes.py` | route table/target checks |
| `scripts/verify/width.py` | router/prose width fence |

The File Map also lists host launcher scripts `scripts/c2python.sh` and `scripts/c2python.cmd`; these are not Python source files.

## Adapter Python surfaces

The File Map separately enumerates Python modules under `adapters/c2modeladapter/**`, including the package implementation, simulation harness, and tests. Those contribute to the broader Python-source tree but should not be conflated with `scripts/**` without using the exact counting rule implemented by `scripts/verify/counts.py`.

## Published measured totals

- Python scripts: **64**
- validators/gates: **37**
- registered checks: **39**
- routes: **45**

The source command `scripts/verify/counts.py --json` is the authoritative measurement owner.

## Verification topology

A reader should distinguish:

1. **leaf validators** — validate one contract or surface;
2. **selftests** — adversarial/regression collections for a subsystem;
3. **aggregate verifier** — invokes the registered release checks;
4. **simulation/fuzz harnesses** — exercise declared state/case spaces;
5. **package/upgrade verification** — path/hash/baseline integrity;
6. **external/live checks** — provider/production evidence, not implied by repository-local PASS.

## Reconciliation requirement

Before this file is advertised as an exhaustive 64-row Python catalog, generate its path set from the release tree using the same inclusion semantics as `counts.py`. CI should verify both:

- exact path-set equality between source Python paths and catalog rows;
- exact numeric equality between generated counts and published count claims.

Never fill missing rows by copying C2Cognitive's 95-script catalog or by inferring filenames from feature names.
