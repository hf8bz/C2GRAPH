# C2GRAPH v1.0.0 — bounded evidence read guide

`v1.0.0` is the **C2GRAPH Version 1 release**, dated **12 August 2026**. Bounded evidence read is part of that release baseline; it is
not a maintenance add-on. The layer controls how repository bytes become agent evidence without replacing goal,
backup, rollback, secret, pre-write, or effect-time authority.

## Canonical command

`<C2PY> scripts/read/core.py --path <PATH>`

Optional controls are `--target`, `--line`, `--char`, `--max-lines`, `--max-bytes`,
`--max-chars-per-line`, and `--json`. `<C2PY>` is the host-resolved launcher documented in
[C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md).

## Physical evidence contract

The default view has three independent ceilings: **2,000 physical lines**, **131,072 normalized UTF-8 output
bytes**, and **2,000 characters per physical line**. The human coordinates remain `(line, char)`, while an emitted v2 continuation additionally binds an exact source-byte
offset, source-generation digest, ceiling-profile digest, monotonic sequence, and prior ephemeral view digest. A
minified one-line file can therefore progress without pretending its line number changed, and later windows do not
repeat prefix location.

Each content row separates two facts that must not be conflated:

- `line_complete`: the text of that physical line is complete in this view;
- `newline_terminated`: the source line actually ended with a newline.

A final line without a newline is complete but not newline-terminated. Mid-line truncation is neither. Blank
physical lines are retained as zero-text rows when needed so pagination never erases line identity.

## Typed outcomes and recovery

The reader distinguishes `CONTENT`, `EMPTY`, `PAST_EOF`, `FORMAT_REDIRECT`, `NOT_FOUND`, `AMBIGUOUS_PATH`,
`REPAIR_SEARCH_BOUNDED`, `REFUSED_SPECIAL`, `PATH_ESCAPE`, `NOT_REGULAR_FILE`, `CHANGED_DURING_READ`, and
`IO_FAILURE`, `STALE_CURSOR`, `PROFILE_MISMATCH`, and `STRATEGY_REVIEW_REQUIRED`; the CLI additionally applies `SECRET_PRONE_REFUSED`. Missing data therefore does not collapse into the
same empty string as a truly empty normalized text file. A BOM-only UTF-8 file normalizes to `EMPTY` at the initial
cursor.

Unicode/invisible-space/quote-glyph repair is conservative. It auto-selects only one unambiguous contained file.
Directory repair inspection is capped; once uniqueness can no longer be proven inside the bounded directory sample,
the result is `REPAIR_SEARCH_BOUNDED`, not a guess. Edit-distance results are advisory suggestions only.

## Filesystem and format safety

Repository containment is resolved before content I/O. The final leaf is opened with `O_NOFOLLOW` where supported,
then verified through `fstat()` as a regular file. Static device paths and descriptor aliases are rejected before
content reads. Plan/census scanner helpers refuse file symlink content and inventory with non-following `lstat()`.
These controls reduce race and escape surfaces; they are **not** a proof that every hostile filesystem or
same-privilege race is impossible.

PDF/image/archive/executable signatures, NUL-bearing inputs, and invalid UTF-8 are `FORMAT_REDIRECT` rather than raw
text. Scanner byte ceilings trim only an **incomplete UTF-8 suffix caused by the ceiling**; malformed UTF-8 inside
the inspected range remains an error instead of being silently dropped. Streaming line counts use the same LF,
CRLF, and CR semantics as the generic reader.

## Freshness and view identity

The reader binds a view to size, `mtime_ns`, `ctime_ns`, inode, and device before/after the read. Drift produces
`CHANGED_DURING_READ` and discards content. The same fingerprint is digested into emitted v2 continuation state;
source-generation mismatch on a later window returns `STALE_CURSOR`, and changed ceiling profiles return
`PROFILE_MISMATCH`. `view_digest` hashes the canonical path, fingerprint, start cursor,
status, partial reason, resume cursor, byte/character counts, and row semantics/text. Two byte-identical snippets
truncated for different reasons therefore do not share a semantic receipt.

There is deliberately **no persistent “model has seen this file” ledger**. C2GRAPH does not own host context after
compaction, and persisting that belief would turn a read into canonical state mutation. Read receipts are ephemeral;
write authority remains operation-specific.

## Scanner integration

`scan/census.py` uses universal-newline streaming counts and bounded textual probes. `scan/plan.py` uses bounded
lexical streams and explicitly reports deferred symlink content. When scan coverage is bounded, absence of a keyword
is **not** promoted into proof that the keyword is absent from the complete artifact.

## Measured comparison: plain whole-file read vs C2Graph

This experiment is intentionally narrow. **Plain/no-repo** means Python
`Path.read_text(encoding="utf-8", errors="strict")` over the whole file with no C2Graph policy. **With C2Graph**
means the optimized Bounded Read v2 implementation in the released v1.0.0 baseline. Three benchmark rounds
(811-813) used five timing repetitions and two `tracemalloc` peak repetitions per reader/fixture/round. Table values
are medians of the per-round medians. These are local Python measurements, not production latency, process RSS,
model-token usage, billing, or task-quality measurements.

| Fixture | Plain returned UTF-8 bytes | C2Graph first-view bytes | First-view reduction | Plain median ms | C2Graph median ms | Plain peak | C2Graph peak | Full reconstruction calls* |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| `blank_heavy.txt` | 2,505 | 2,000 | 20.16% | 0.008 | 3.235 | 0.01 MiB | 1.12 MiB | 2 |
| `cr_only.txt` | 18,000 | 12,000 | 33.33% | 0.019 | 6.880 | 0.06 MiB | 1.24 MiB | 2 |
| `long_80000.txt` | 7,760,000 | 131,072 | 98.31% | 4.232 | 5.245 | 14.81 MiB | 1.19 MiB | 60 |
| `minified_512k.js` | 524,299 | 2,000 | 99.62% | 0.074 | 0.145 | 1.01 MiB | 0.14 MiB | not forced |
| `minified_64k.js` | 65,547 | 2,000 | 96.95% | 0.014 | 0.122 | 0.13 MiB | 0.14 MiB | 33 |
| `no_final_newline.txt` | 25 | 25 | 0.00% | 0.006 | 0.103 | 0.01 MiB | 0.01 MiB | 1 |
| `ordinary_3000.txt` | 243,000 | 131,072 | 46.06% | 0.039 | 6.080 | 0.47 MiB | 1.35 MiB | 2 |
| `tiny.txt` | 11 | 11 | 0.00% | 0.006 | 0.110 | 0.01 MiB | 0.01 MiB | 1 |
| `unicode.txt` | 80,000 | 32,000 | 60.00% | 0.111 | 9.067 | 0.33 MiB | 1.84 MiB | 3 |
| `wide_400.log` | 2,000,400 | 2,000 | 99.90% | 0.231 | 0.143 | 3.82 MiB | 0.09 MiB | not forced |

\* `not forced` means the benchmark intentionally stops complete pagination for hostile shapes and expects a
strategy switch instead. For finite reconstruction rows, exact normalized reconstruction matched the source in all
three rounds.

### Interpretation

- Small and ordinary sub-megabyte files remain a **negative latency result** for C2Graph because containment,
  fingerprinting, typed rows, cursor receipts, and generation binding cost work. Bounded Read v2 is not a universal
  speed optimization.
- The optimized implementation removes the earlier overlapping-read regression: on `ordinary_3000.txt`, the same
  harness fell from about 53.8 ms before the read-ahead/O(1)-counter correction to about 6.08 ms after it.
  The comparison baseline is supplemental engineering evidence, not a release performance claim.
- The 7.76 MB ordinary fixture admits 131,072 bytes on the first view (98.31% less than whole-file returned bytes)
  while keeping local `tracemalloc` peak below the whole-file baseline in this harness.
- The 512 KiB minified and 2.00 MB wide-line fixtures admit only 2,000 bytes because the per-line ceiling fires.
- First-view byte reduction is a transport/evidence-content measurement. It must not be relabeled as token savings,
  billing savings, reasoning improvement, or developer productivity.

### Peak-allocation scale sweep

A separate optimized-v2 ordinary ASCII-line sweep used rounds 821-823, seven timing repetitions and two
`tracemalloc` peak repetitions per reader/size/round. On this measured grid, C2Graph first becomes lower than plain
whole-file `tracemalloc` peak at **1,048,576 bytes**. This crossover is harness-, runtime-, and fixture-specific.

| Input bytes | Plain median ms | C2Graph median ms | Plain peak | C2Graph peak | C2/Plain peak |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 32,768 | 0.009 | 1.557 | 0.07 MiB | 0.35 MiB | 5.081x |
| 65,536 | 0.013 | 3.116 | 0.13 MiB | 0.68 MiB | 5.213x |
| 131,072 | 0.021 | 6.082 | 0.26 MiB | 1.35 MiB | 5.292x |
| 262,144 | 0.038 | 6.137 | 0.51 MiB | 1.35 MiB | 2.677x |
| 524,288 | 0.067 | 6.123 | 1.01 MiB | 1.35 MiB | 1.346x |
| 1,048,576 | 0.124 | 6.107 | 2.01 MiB | 1.35 MiB | 0.675x |
| 2,097,152 | 0.999 | 6.133 | 4.01 MiB | 1.35 MiB | 0.338x |
| 4,194,304 | 2.059 | 6.130 | 8.01 MiB | 1.35 MiB | 0.169x |
| 8,388,608 | 4.800 | 6.167 | 16.01 MiB | 1.35 MiB | 0.085x |
| 16,777,216 | 10.739 | 6.169 | 32.01 MiB | 1.35 MiB | 0.042x |

### Safety/semantics matrix

| Case | Plain whole-file result | C2Graph result | Same result in all 3 rounds |
| --- | --- | --- | --- |
| `directory` | `EXCEPTION` | `NOT_REGULAR_FILE` | yes |
| `document.pdf` | `CONTENT` | `FORMAT_REDIRECT` | yes |
| `fifo` | `BLOCKED_GT_200MS` | `NOT_REGULAR_FILE` | yes |
| `invalid_utf8.bin` | `EXCEPTION` | `FORMAT_REDIRECT` | yes |
| `outside_symlink` | `CONTENT` | `PATH_ESCAPE` | yes |
| `repair_directory_bound` | `EXCEPTION` | `REPAIR_SEARCH_BOUNDED` | yes |
| `secret_prone` | `CONTENT` | `SECRET_PRONE_REFUSED` | yes |
| `typo_path` | `EXCEPTION` | `NOT_FOUND` | yes |
| `unicode_filename_nfc_nfd` | `EXCEPTION` | `CONTENT` | yes |
| `valid_nul.bin` | `CONTENT` | `FORMAT_REDIRECT` | yes |

The FIFO baseline was executed in a child process with a 200 ms timeout rather than allowed to hang. C2Graph
classified it as non-regular without content blocking. The raw plain secret fixture returns content; the C2Graph CLI
refuses it before returning the secret value.

## Bounded Read v2 hardening

The released v1.0.0 baseline keeps the three original per-view ceilings and adds a second bound at read-intent level.
Three sequential windows are permitted by default; a fourth blind page returns `STRATEGY_REVIEW_REQUIRED`. The
caller must switch to search/range/tail/parser/stream evaluation or explicitly justify continued sequential inspection.
The threshold is a shipped policy default, not an empirically proven optimum.

Generated continuation cursors seek directly from their exact source offset. Legacy arbitrary `(line,char)` starts
still need a bounded scan to locate the first source byte, so the v1.0.0 release does not claim O(1) arbitrary seek or
universal latency improvement. The generation digest detects the tested local source drift classes; it is not a
digital signature, immutable snapshot, or proof against all filesystems and same-user races.

## Validation and reproducibility boundary

The final reader self-test contains **56 deterministic cases per language**. Three fresh reviewer rounds execute 20
roles per language (40 process jobs per round), and all three optimized-v2 rounds completed **40/40**, for **120/120
process jobs**. These are deterministic/process reviewers, not independent LLM-agent trials.

A separate fixed-seed refutation on the exact optimized reader completed **3,000/3,000 exact reconstructions**,
**3,000/3,000 valid arbitrary legacy starts**, **400/400 cross-window mutation cases with stale-generation
rejection**, **500/500 stable concurrent reads**, and **3,000 leaf-swap attempts with zero outside-root bytes
observed**. Passing means no defect was observed in those enumerated local cases; it does not prove universal bug
absence, production token reduction, optimal ceiling values, or protection from every filesystem implementation.

## Final-code refutation summary

Bounded Read v2 closes two earlier contract gaps without changing write authority. Generated continuations are
source-generation bound and seek from exact source offsets, while a default three-window sequential budget forces
strategy review before a fourth blind page. Legacy `(line,char)` starts remain compatible but may scan once to locate
the first exact source byte. Hard ceilings may be narrowed by ordinary callers but not widened. The separate
cross-window mutation suite and repeated process-reviewer rounds verify these contracts on the tested tree; the
fingerprint is still an integrity/freshness mechanism, not a digital signature or immutable filesystem snapshot.
