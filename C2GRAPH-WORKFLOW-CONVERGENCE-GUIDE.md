# Workflow Convergence Guide - v1.3.2

CORE-40 bounds recovery on one semantic frontier:

`NO_PROGRESS_OBSERVED -> DIAGNOSE_ONCE -> STOP_BUSY_LIVELOCK`

A closed-list invalidator can legally reset the convergence budget. External wait remains evidence-bound, completion
is sticky, and verification reuse remains PASS-only. v1.3.2 adds a separate terminal cross-plane gate: semantic
convergence decides whether recovery should continue, while terminal reconciliation decides whether durable continuity
is allowed to treat a completed run as resumable.

A terminal predecessor is never resumed. A successor is admitted only after independently verified completion and one
globally unique evidence-bound claim. `terminal_reconcile.py` default mode is the resume gate; `--successor-json FILE`
is successor admission. Both are read-only. The existing state lock/fence provides serialization at append time.
