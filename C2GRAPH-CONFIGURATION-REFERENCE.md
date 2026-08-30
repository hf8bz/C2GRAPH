# C2GRAPH Configuration Reference

**Bahasa Indonesia:** [C2GRAPH-CONFIGURATION-REFERENCE-ID.md](C2GRAPH-CONFIGURATION-REFERENCE-ID.md)

> Public index for C2GRAPH v1.2.0 configuration. `.agent/config.yml` is the runtime/configuration owner. This document is descriptive and must be regenerated or audited when the source config changes.

## 1. Configuration principles

C2GRAPH centralizes tunable values so the same threshold is not duplicated across unrelated prose. Configuration does not override semantic scope, exact write authority, rollback requirements, or fresh pre-state.

The current public Reference exposes configuration across these families:

- project/stack binding;
- commands and paths;
- graph/search/edit tooling;
- mobile binding and QA matrix;
- routing/read/retrieval boundaries;
- agent output and concurrency;
- batch control;
- lessons/failure recall;
- ratchet loops;
- graph-write and tool-call budgets;
- compaction/handoff/resume;
- interactive question policy;
- tool discovery/execution policy;
- provenance;
- compliance mapping;
- rule preservation.

## 2. Project and tool binding keys exposed publicly

The Reference currently names these project/tool bindings:

`name`, `root`, `shell`, `primary_language`, `package_manager`, `orm`, `database`, `build`, `test`, `lint`, `deploy`, `migrations`, `tracker`, `product_doc`, `design_doc`, `production`, `staging`, `graph`, `search`, `find`, `struct_edit`, `loc_count`, `json_query`, `work_graph`, Android module binding, iOS scheme binding, and `qa_matrix`.

Template placeholders are not valid instantiated values when a gate requires a real project binding.

## 3. Publicly documented thresholds and controls

The current public Reference documents at least the following settings and defaults/semantics:

| Key | Public default / meaning |
| --- | --- |
| `slice_specificity_min` | `0.6` |
| `router_max_lines` | `170` |
| `subagent_max_output_words` | `600` |
| `subagent_wait_seconds_min` | `30` |
| `batch_max_items` | `20` |
| `batch_trigger_files` | `5` |
| `lesson_unconfirmed_decay_days` | `60` |
| `lesson_location_claim_confidence_cap` | `0.4` |
| `ratchet_max_rounds` | `3` |
| `max_subagents_per_run` | `16` |
| `max_concurrent_workers` | `8` |
| `max_tool_calls_per_run` | `300` per execution/resource epoch |
| `max_retries_per_step` | `2` |
| `max_graph_writes_per_run` | `500` |
| `false_merge_review_min_confidence` | `0.8` |
| `compaction_warn_ratio` | `0.7` |
| `compaction_forced_ratio` | `0.85` |
| `handoff_checkpoint_every_steps` | `25` |
| `handoff_max_tool_calls` | `8` |
| `rehydrate_max_tokens` | `2000` |
| `resume_block_max_lines` | `20` |
| `handoff_autoresume` | `true` |
| `question_interactive` | `true` |
| `question_option_min` | `2` |
| `question_option_max` | `5` |
| `question_freeform_slot` | `true` |
| `question_chain_without_stop` | `true` |
| `question_batch_mode` | `true` |
| `question_batch_min` | `3` |
| `question_batch_writer` | `scripts/interview/ask.py` |
| `question_emit_when_no_tty` | `true` |
| `question_batch_writer_emit_flag` | `--emit` |
| `scan_scripts_execution_allowed` | `true` |
| `scan_scripts_output_is_evidence` | `true` |
| `tool_verification_timeout_seconds` | `60` |
| `tool_verification_cold_timeout_seconds` | `600` |
| `tool_verification_poll_seconds` | `15` |
| `tool_verification_retries` | `1` |
| `tool_discovery_exec_allowed` | `false` |
| `tool_discovery_cache_write_allowed` | `false` |
| `tool_discovery_manifest_write_allowed` | `false` |
| `autoresume_crosses_phase_boundary` | `false` |
| `autoresume_question_policy` | `blocker_or_sensitive_only` |
| `autoresume_deferred_max` | `10` |
| `goal_max_lines` | `120` |
| `stage_goal_max_lines` | `60` |
| `discipline_max_repeat_violations` | `3` |
| `toolmap_min_confidence` | `0.7` |
| `toolmap_generate_max` | `40` |
| `doc_derived_confidence_cap` | `0.6` |
| `contradiction_stop_threshold` | `10` |
| `provenance_export_enabled` | `true` |
| `provenance_required_fields_enforced` | `true` |
| `provenance_digest_algorithm` | `sha256` |
| `provenance_signature_required` | `false` |
| `provenance_actor_unknown_allowed` | `false` |
| `provenance_example_min` | `3` |
| `compliance_map_enabled` | `true` |
| `compliance_frameworks_declared` | `3` |
| `compliance_rows_min` | `30` |
| `compliance_unmapped_core_allowed` | `2` |
| `compliance_evidence_required` | `true` |
| `compliance_control_id_pattern_enforced` | `true` |
| `rule_preservation_enforced` | `true` |
| `core_baseline_digest` | set at install |
| `core_baseline_path` | `.agent/core-baseline.md` |
| `rule_registry_path` | `.agent/rule-registry.md` |
| `rule_rejection_requires_approver` | `true` |
| `rule_rejection_max` | `0` |
| `router_absorb_mode` | `scope_first` |

## 4. Important interpretation rules

- A numeric threshold is not write permission.
- Increasing an agent/concurrency/tool-call budget does not increase semantic scope.
- Cache/discovery settings do not make cached material authoritative.
- `handoff_autoresume=true` does not authorize crossing a guarded phase boundary.
- `provenance_signature_required=false` means the shipped digest chain is not a key-based identity signature.
- Compliance mappings remain evidence mappings, not certification.

## 5. Completeness note

This public reference deliberately lists the keys already exposed by the current public C2GRAPH Reference. Before advertising this file as an **exhaustive key-for-key index**, generate an exact key inventory from the shipped `.agent/config.yml` and add a CI check that compares the public catalog against that inventory. Do not infer missing keys from C2Cognitive or any other repository.
