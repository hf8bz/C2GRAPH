# C2GRAPH Install, Adopt and Bootstrap Guide

**Bahasa Indonesia:** [C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE-ID.md](C2GRAPH-INSTALL-ADOPT-BOOTSTRAP-GUIDE-ID.md)

> C2GRAPH v1.2.0 lifecycle guide for selecting and executing the correct repository entry path.

## 1. Formal entry modes

C2GRAPH defines three formal entry modes:

- `ADOPT_WITH_GOAL`
- `WORKFLOW_ONLY`
- `BOOTSTRAP`

Do not use `SCAN` as a fourth formal mode. `SCAN-AND-ADOPT.prompt.md` is the prompt that directly selects `ADOPT_WITH_GOAL` for an existing repository.

## 2. Decision flow

```text
Is the target repository empty/new?
  yes -> BOOTSTRAP -> BOOTSTRAP-NEW-REPO.prompt.md
  no  -> Does this session need to change product behavior?
           yes -> goal-bound path; for adoption use SCAN-AND-ADOPT.prompt.md
           no  -> Is the request only C2GRAPH install/repair/upgrade/verification?
                    yes -> WORKFLOW_ONLY -> INSTALL-WORKFLOW-ONLY.prompt.md
                    no  -> task is underspecified/mixed; classify it before acting
```

## 3. Source root versus target root

Keep the C2GRAPH distribution/source distinct from the target project. Reading a prompt/script from the C2GRAPH source does not authorize copying it into the target. Target mutation waits for the canonical rollback/write-plan authority path.

## 4. Workflow-only hard boundary

`WORKFLOW_ONLY` may alter only the authorized C2GRAPH control-plane installation/repair/upgrade/verification set. It must not:

- change product behavior;
- update business logic;
- modify product migrations/tests/features;
- emit/reactivate/repair logical goals as a side effect;
- resume an active product goal automatically.

Goal records are preserved and reported.

## 5. Adoption with goal

Direct `SCAN-AND-ADOPT.prompt.md` execution is appropriate for an existing repository when adoption is intended to finish with a new goal-bound implementation path. Discovery begins read-only with respect to target bytes; later mutation still requires exact write authority.

## 6. Bootstrap

Bootstrap is for an empty/new repository. It establishes the C2GRAPH governance substrate and project bindings through the bootstrap contract. It is not a shortcut for an existing repository whose product semantics must first be discovered.

## 7. Rollback/write authority

Entry-mode selection decides **which workflow contract applies**. It does not itself authorize target mutation. Actual mutation remains governed by semantic scope, exact PHASE 3.W plan, rollback basis, fresh pre-state, and immediate verification.

## 8. After installation

Do not re-run installation entry prompts for ordinary product sessions. Once C2GRAPH is installed, normal product work proceeds under a suitable active goal and applicable scopes/runbooks/gates.
