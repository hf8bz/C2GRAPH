# C2GRAPH CLI Launcher Guide

## Purpose

C2GRAPH ships Python 3 utilities, but **`python3` is not a portable command name**. Windows commonly
provides the Python Launcher as `py -3` or exposes `python.exe`; some POSIX hosts expose `python3`,
others expose `python`. C2GRAPH therefore uses one canonical abstraction: **`C2PY`**.

No operator/agent instruction should hard-code `python3` as the executable used to run a shipped
C2GRAPH script.

## Canonical launcher

Resolve `C2PY` once per CLI/session from the trusted `C2GRAPH_SOURCE_ROOT`:

| Host | Canonical command |
| --- | --- |
| POSIX shell | `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"` |
| Windows cmd.exe | `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"` |
| Windows PowerShell | `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"` |

Every C2GRAPH Python example is then written as:

```text
<C2PY> scripts/verify/all.py
<C2PY> scripts/scan/tooling.py --target <TARGET_ROOT>
<C2PY> scripts/install/preflight.py --template <C2GRAPH_SOURCE_ROOT> --target <TARGET_ROOT>
```

`<C2PY>` is documentation notation for the complete host-specific **invocation prefix** above; it is not typed literally and is not a shell variable that should be expanded with `eval`. Quoting is part of the canonical form so a source root containing spaces remains valid.

## Resolution order

Both wrappers require Python **3.8 or newer**. The POSIX wrapper uses:

1. explicit `C2GRAPH_PYTHON` when configured;
2. `python3` when present;
3. `python` when present;
4. otherwise exit 127 with `C2PY_ERROR code=python_runtime_unavailable_or_unsupported`.

The Windows wrapper uses:

1. explicit `%C2GRAPH_PYTHON%` when configured;
2. `py.exe -3`;
3. `python.exe`;
4. `python3.exe`;
5. otherwise exit 127 with the same closed error class.

The wrappers validate the selected runtime before launching a C2GRAPH script and disable Python bytecode generation so simply running a shipped read-only scanner does
not create `__pycache__` inside the distribution/target.

## Why this is safer than probing every candidate

GATE 0 still forbids arbitrary tool probes. `C2PY` is different: Python execution is required to run
the shipped C2GRAPH utility the user already requested. The wrapper resolves the runtime **at the
moment Python execution is needed**; it does not execute project package managers, network runners,
or product binaries to discover availability.

`scan/tooling.py` no longer reports a literal tool named `python3`. It reports `python-runtime` from
`sys.executable`, because the interpreter currently executing the scanner is the execution proof.

## Session restart rule

`C2PY` is **host/session state**, not repository state. Resolve it again after:

- CLI restart;
- machine change;
- container change;
- IDE/terminal change;
- budget/resource suspension resumed through Prompt 97.

A resume capsule never assumes that the old Python command spelling remains valid.

## Failure handling

If neither launcher can find Python 3:

- do not substitute `npx`, package-manager bootstrap, download/install, or an unrelated interpreter;
- report `BLOCKER class tool_absent` only if the next required C2GRAPH action genuinely needs Python;
- continue independent non-Python read-only work where the phase allows it;
- installation of Python is a separate human-approved tooling decision.

A failed literal `python3` invocation such as Windows exit `9009` should not occur in the canonical
path because C2GRAPH never asks the host to use that spelling directly.

## Audit requirement

Release audit scans EN, ID, Docs, and Flowchart source for hard-coded `python3` CLI instructions.
Occurrences are allowed only inside the launcher implementation itself or prose explaining fallback
candidates. Normative command examples must use `<C2PY>`.
