# C2GRAPH Prompt Catalog

**Bahasa Indonesia:** [C2GRAPH-PROMPT-CATALOG-ID.md](C2GRAPH-PROMPT-CATALOG-ID.md)

> C2GRAPH v1.2.0 public prompt map. Prompt files are executable/operator-facing contracts in the release distribution; this catalog is explanatory.

## 1. Root entry prompts

| Prompt | Formal mode / role | Mutation boundary |
| --- | --- | --- |
| `SCAN-AND-ADOPT.prompt.md` | direct existing-repo adoption => `ADOPT_WITH_GOAL` | begins read-only; later writes require canonical exact authority |
| `INSTALL-WORKFLOW-ONLY.prompt.md` | `WORKFLOW_ONLY` | C2GRAPH control-plane only; product behavior and goal mutation forbidden |
| `BOOTSTRAP-NEW-REPO.prompt.md` | `BOOTSTRAP` | new/empty repo path; later writes remain governed |

## 2. Staged graph-engineering prompts publicly enumerated by the File Map

| Prompt | Role |
| --- | --- |
| `prompts/00-baseline-audit.prompt.md` | read-only baseline audit |
| `prompts/01-measurable-loop.prompt.md` | establish measurable iteration loop |
| `prompts/02-lessons-corpus.prompt.md` | build/use lessons corpus under governance |
| `prompts/03-graph-plane.prompt.md` | introduce graph plane after prerequisites/maturity |
| `prompts/04-context-split.prompt.md` | context/corpus split with evidence boundaries |
| `prompts/05-retrieval-eval.prompt.md` | retrieval evaluation after partitions exist |
| `prompts/06-operations.prompt.md` | operationalization after baseline/context prerequisites |
| `prompts/07-compaction-drill.prompt.md` | deliberate handoff/compaction rehearsal |
| `prompts/08-goal-emit.prompt.md` | emit an implementation goal after requirements readiness |
| `prompts/97-budget-resume.prompt.md` | resume `suspended_resumable` work in a new resource epoch |
| `prompts/98-compaction-handoff.prompt.md` | capture handoff around compaction risk |
| `prompts/99-session-resume.prompt.md` | continue long-running work after session boundary |

## 3. Prompt selection rules

1. Installation entry prompts are not ordinary product-session prompts.
2. `SCAN-AND-ADOPT` is a prompt name; the formal mode is `ADOPT_WITH_GOAL`.
3. Workflow-only may install/repair/upgrade/verify the C2GRAPH control plane but cannot change product behavior or logical goal state.
4. Resume prompts restore bounded continuity; they do not recreate lost authority from memory.
5. A staged prompt prerequisite is part of its contract. Do not reorder stages merely because a later prompt is attractive.
6. A prompt may produce evidence, plans, or state, but repository writes still require the applicable canonical write predicate.

## 4. Coverage control

Before publication as an exhaustive catalog, compare this list against the actual release-tree prompt path set and `.agent/distribution-files.txt`. CI should fail if a shipped `*.prompt.md` path lacks a catalog row or if a catalog row names a non-shipped prompt.
