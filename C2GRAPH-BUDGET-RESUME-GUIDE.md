# C2GRAPH Budget Suspension & Resume Guide
> **CLI notation:** `<C2PY>` is documentation notation, not a literal command or shell assignment.
> Resolve it for the current host/session using [C2GRAPH-CLI-LAUNCHER-GUIDE.md](C2GRAPH-CLI-LAUNCHER-GUIDE.md):
> - POSIX: `/bin/sh "<C2GRAPH_SOURCE_ROOT>/scripts/c2python.sh"`
> - Windows cmd.exe: `"<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> - Windows PowerShell: `& "<C2GRAPH_SOURCE_ROOT>\scripts\c2python.cmd"`
> Re-resolve after a CLI/session/host restart; quoting is part of the canonical prefix.

## Problem this contract prevents

A run can finish PHASE 3 correctly and then lose the CLI because the tool-call, token/context, rate,
or host-session budget is exhausted. If the protocol waits until the hard limit before writing a
handoff, the run may close with no usable continuation state. Restarting SCAN from PHASE 0 then wastes
verified work and can produce different decisions.

C2GRAPH v1.2.0 continues to treat resource exhaustion as a **resumable suspension**, not a terminal task
failure.

## Two boundaries, not one

For normal SCAN:

- hard cap: `300` tool calls;
- reserved continuity budget: `12` calls;
- soft handoff boundary: `288` used calls.

For BOOTSTRAP:

- hard cap: `200`;
- reserve: `12`;
- soft handoff boundary: `188`.

The mechanical classifier is:

```text
<C2PY> scripts/budget/check.py --cap <cap> --used <used> --reserve 12 --handoff-cost 8
```

It returns one of three states:

| State | Meaning | Action |
| --- | --- | --- |
| `CONTINUE` | reserve is still intact | continue already-authorized work |
| `HANDOFF_NOW` | remaining budget is inside the reserve | start no new unit; emit capsule; suspend |
| `HARD_EXHAUSTED` | cap is already reached | run no tool; emit text-only capsule; suspend |

A stricter host token/context/rate-limit warning wins and can trigger suspension earlier.

## PHASE 3 is always checkpointed

After PHASE 3/3.W settles the decisions/write plan and **before PHASE 4**, the agent must emit a fresh
`C2GRAPH_RESUME_CAPSULE v1` into user-visible output even when the numeric budget is healthy.

This solves the specific failure mode "PHASE 3 completed, then the CLI closed". The next CLI can
resume from Prompt 97 without re-running PHASE 0-3.

The capsule is target-independent. It does not require `_decisions.md`, `.agent/state/**`, or any
extra target write. Durable target persistence remains subject to exact post-3.W semantic scope,
write-plan membership, path-scoped rollback/write authority, and per-action freshness.

## Required suspension report

A budget/resource boundary must report:

```text
RUN_OUTCOME      : suspended_resumable
SUSPEND_REASON   : budget_soft_boundary | budget_hard_exhausted | context_pressure | rate_limit | host_session_end
PROGRESS         : <exact completed/total>
CONTINUATION     : RESUME_REQUIRED
NEXT_SAFE_ACTION : <exact action>
```

and then the complete capsule from `.agent/resume-capsule.schema.md`.

Do **not** emit `RUN_OUTCOME: stopped` merely because the budget is depleted. Do not consume a retry.
Do not invent an estimated percentage.

## What if no tool call remains?

The emergency fallback uses **zero additional tools**. The agent prints the capsule from evidence
already in context. It does not run Python, Git, a scanner, or a validator just to checkpoint.

Therefore "Python is unavailable at the exact moment of hard exhaustion" does not make the run
unresumable. `C2PY` is resolved again in the new session.

## Resume order

A new session prefers:

1. valid durable target handoff/cursor;
2. pasted transcript/external-sidecar `C2GRAPH_RESUME_CAPSULE v1`;
3. verified reconstruction labelled `CHECKPOINT_SOURCE: reconstructed`.

Absence of `.agent/state/**` no longer means "there was no checkpoint". A transcript capsule is a
real checkpoint.

Use `prompts/97-budget-resume.prompt.md`. It echoes the capsule, re-resolves `C2PY`, checks only the
freshness/divergence required for the exact next action, and continues from `NEXT_ACTION`. It must not
restart adoption/bootstrap or re-ask settled PHASE 3 questions.

## Related failure classes

- `deferred_budget`: still an `AWAIT` human boundary when too many questions were defaulted, but it
  carries a resume capsule.
- `handoff_max_tool_calls`: if durable handoff would exceed 8 calls, emit the minimal capsule instead;
  do not spend the remaining run budget on non-essential checkpoint refinement after the required continuation evidence is complete.
- context compaction/forced ratio: produce capsule + resumable suspension, then compact/resume.
- rate limit: suspend instead of busy-retrying.
- unexpected host termination: the newest already-emitted phase/checkpoint capsule is authoritative;
  only an explicitly in-progress unit may be repeated.

## Files that implement the contract

- `.agent/runbooks/budget-resume.md`
- `.agent/resume-capsule.schema.md`
- `prompts/97-budget-resume.prompt.md`
- `scripts/budget/check.py`
- `scripts/handoff/capsule.py`
- `.agent/runbooks/context-compaction.md`
- `docs/graph-engineering/11-complexity-budget.md`

## Resource epoch reset prevents a resume loop

`max_tool_calls_per_run` is the cap of one execution/resource epoch. A resumed CLI is a new execution
run/epoch for resource accounting, while the logical workflow state continues unchanged. Prompt 97
therefore increments `BUDGET_EPOCH`, sets local `BUDGET_USED` to `0`, and carries
`BUDGET_CUMULATIVE_USED` only for audit. Never pass the exhausted prior epoch's local usage to the new
epoch classifier; doing so would produce an infinite `HARD_EXHAUSTED -> resume -> HARD_EXHAUSTED` loop.
