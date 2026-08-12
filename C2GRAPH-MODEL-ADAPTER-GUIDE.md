# C2GRAPH Model Adapter and Cache-Aware Boundary Guide

**Product release:** C2GRAPH v1.0.0 — Version 1 / first public release.  
**Engineering adapter revision:** C2ModelAdapter v0.4.1.  
**Scope:** host-side protocol/runtime/cache compatibility. The adapter does not become C2GRAPH write authority.

## 1. Why this layer exists

C2GRAPH governs repository work, goals, evidence, handoff, rollback and mutation authority. Provider-specific model wire formats are a different problem. The model adapter therefore stays at the host boundary and translates provider/runtime behavior into bounded, provider-neutral capability evidence before a worker is admitted.

This separation avoids two unsafe shortcuts: treating an MCP/tool transport as a model parser, and treating a provider cache hit as proof that a C2GRAPH action is authorized or correct.

## 2. What the adapter normalizes

The adapter can normalize structured tool calls, provider/runtime model identifiers, native tool-call identifiers, streaming completion assembly requirements, reasoning-state preservation requirements, JSON Schema argument validation, runtime parser selection, and cache-plan hints. Unknown model/runtime combinations fail closed unless the host supplies a verified explicit profile.

C2GRAPH receives only capability evidence needed for admission. Raw API keys, raw provider cache-routing keys, raw prompt prefixes, cache contents and private reasoning state are outside durable C2GRAPH state.

## 3. Cache authority boundary

A cache is an optimization. A cache miss must remain a correct execution path. `cache_hit`, cached-token counts, cache-write counts and provider routing keys cannot satisfy a goal gate, evidence gate, completion gate, rollback gate or write-authority gate.

The host binds cache reuse to a stable-prefix digest and an opaque scope digest. Those digests may be admitted as evidence; the underlying prefix and provider key are not persisted as C2GRAPH authority.

## 4. Current provider/runtime contracts verified for this release

The adapter intentionally keeps separate provider contracts instead of inventing one universal cache flag.

- **OpenAI GPT-5.6 family:** supports automatic prefix caching, `prompt_cache_key`, explicit breakpoints, and current `prompt_cache_options` behavior used by this adapter. The adapter keeps future model families conservative until separately revalidated.
- **Azure OpenAI GPT-5.6 family:** Standard pay-as-you-go supports the current cache-option/breakpoint family; PTU-M does not support breakpoints. `deployment_mode` therefore participates in the cache plan and required-cache mode fails closed when the runtime evidence is insufficient.
- **xAI Grok:** Responses uses `prompt_cache_key`; Chat Completions uses `x-grok-conv-id`. This reference adapter emits only the key appropriate to the selected interface.
- **Anthropic:** uses provider `cache_control`; the adapter permits the documented 5-minute and 1-hour TTL values.
- **Gemini Interactions:** treated as implicit caching. The adapter does not invent an explicit request field on this interface.
- **vLLM/open-weight runtimes:** runtime prefix caching must be explicitly declared when cache capability is required. Runtime/parser identity is scoped to the serving runtime, not treated as a universal model property.
- **Meta Model API / Muse-family profile:** provider-managed cached-input behavior is represented without inventing an unverified cache-control field.

Provider contracts can change. The shipped profile is evidence for the audited date, not a promise that a future provider API will preserve the same fields.

## 5. Admission contract

The C2GRAPH capability validator accepts schema generations v1-v3 for compatibility. Production admission can require all of: live probe evidence, cache-aware evidence, and a verified runtime contract. Streaming is rejected when the capability document does not declare complete-turn assembly. Required native-ID and reasoning-state preservation must match observations.

A canonical SHA-256 digest may bind an admitted capability document. The digest proves equality to the admitted document, not semantic correctness of the model's decision.

## 6. Verification commands

Resolve `<C2PY>` through `C2GRAPH-CLI-LAUNCHER-GUIDE.md`, then run:

```text
<C2PY> scripts/selftest/model_adapter.py
<C2PY> scripts/verify/adapter.py
<C2PY> scripts/verify/all.py
```

Standalone adapter engineering tests are under `adapters/c2modeladapter/`. They are reference/engineering evidence and remain distinct from C2GRAPH authority.

## 7. Measured integration snapshot

Per language edition in the adapter-integrated v1.0.0 tree: 198 distributed files, 46 shipped Python scripts, 32 validators/gates, 30 registered checks, 43 routing rows, 36 runbooks including the template, and 24 graph-engineering documents. CORE remains contiguous at CORE-01..CORE-38.

The model-adapter selftest executed 23/23 cases per edition. The standalone reference adapter executed 33 pytest cases, 20,000/20,000 randomized structural cases and 10,000/10,000 randomized cache-plan cases in the recorded audit. These are protocol/process tests, not a live paid-provider benchmark.

## 8. Non-claims

The adapter is not claimed to improve semantic reasoning, choose the correct tool, guarantee provider cache hits, prove lower production latency/cost, or provide a security boundary against a hostile host. In the explicit negative control, semantic wrong-tool detection remains 0% before and after because a schema-valid but semantically wrong choice is outside the adapter's role.

## 9. Primary provider documentation captured for the audited contract

- OpenAI prompt caching: `https://platform.openai.com/docs/guides/prompt-caching`
- Azure OpenAI prompt caching: `https://learn.microsoft.com/azure/ai-foundry/openai/how-to/prompt-caching`
- xAI prompt caching: `https://docs.x.ai/developers/advanced/prompt-caching`
- Anthropic prompt caching: `https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching`
- Gemini caching: `https://ai.google.dev/gemini-api/docs/caching`
- vLLM automatic prefix caching: `https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/`

The release evidence records the audit date and claim boundary; operators should revalidate provider documentation before changing cache fields.
