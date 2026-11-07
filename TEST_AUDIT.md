# Unit test audit

Removed tests are listed below individually. Behavioral protocol, persistence, permissions, scheduling, cancellation, timeout, parsing, accounting and UI geometry tests are retained. Mixed tests retain their behavioral portions; this is not a blanket removal of default/configuration coverage. No tests read README.md to assert endpoint configuration (the README filename in shell parsing fixtures is ordinary test input).

- `ApplicationTests.test_semantic_missing_tool_recovery_is_completely_removed` — Scans the entire source for retired helper names and prose; transport/runtime failure tests exercise the actual behavior.
- `ApplicationTests.test_no_missing_tool_classifier_or_recovery_prompt_remains` — Duplicates the retired-helper source scan and pins error-message prose rather than recovery behavior.
- `LongStreamingTests.test_streaming_and_compaction_sources_use_rope_accumulation` — Pins local variable spellings and concatenation syntax; long-stream and accumulator behavior tests remain.
- `ResponsesAndTaskInspectionTests.test_endpoint_editor_exposes_api_dropdown` — Pins tooltip prose, widget IDs and a content-height literal; protocol selection/serialization and editor round-trip tests remain.
- `ResponsesAndTaskInspectionTests.test_endpoint_editor_height_is_twenty_percent_taller` — Pins a historical size ratio and exact source statements rather than usable/clipped geometry.
- `EndpointSettingsAndChatRenderingTests.test_new_chat_settings_layout_plain_task_label_and_spacing` — Pins drawing implementation strings and historical width arithmetic; behavior/default permission tests replace these assertions.
- `SharedRecordsAndRuntimeCachesTests.test_runtime_fifo_collections_are_deques` — Asserts container spellings and private method names; scheduler ordering/concurrency and report-delivery tests remain.
- `ContextCapacityAndIdleWorkTests.test_task_components_and_entry_points_are_documented` — Searches prose and code for a vocabulary blacklist/whitelist; not an execution or documentation correctness test.
- `ToolPolicyAndImageExecutionTests.test_toolbar_format_has_status_and_no_leading_or_trailing_pipe` — Asserts exact f-string spelling and centering expression; does not render or exercise toolbar output.
- `ToolPolicyAndImageExecutionTests.test_tool_defaults_card_reserves_all_five_toggle_rows` — Pins source row counts and an obsolete always-on checklist assumption; new-chat permission tests cover the requested defaults.
- `EndpointRecoveryTests.test_endpoint_editor_and_live_tool_preview_are_exposed` — Checks unrelated labels/private source fragments in one test; request and tool-preview behavior tests remain.
- `TranscriptScrollingAndToolRowsTests.test_image_analysis_returns_endpoint_text_without_semantic_parsing` — Checks source syntax and banned private helper names rather than returned image-analysis results.
- `TranscriptScrollingAndToolRowsTests.test_image_analysis_contract_contains_no_history_or_reconciliation_helpers` — Source-wide blacklist of retired names, redundant with image-analysis behavioral coverage.
- `RateLimitEncodingAndDecisionTests.test_opening_docs_cover_binary_capture_rate_defaults_and_cooldown_stats` — Couples unrelated default inventory, old release text and source spellings; dedicated recovery/schema/usage tests remain.
- `EngineeringWorkflowTests.test_production_source_has_no_removed_red_tape` — Source-wide prose blacklist can fail harmless comments and cannot establish runtime behavior.
- `EngineeringWorkflowTests.test_opening_documentation_states_the_final_philosophy` — Pins generic words in guidance prose; no functional or meaningful documentation invariant.
- `UnifiedEndpointEditorTests.test_default_card_description_measurement_matches_draw` — Checks identifier spelling in two functions and globally forbids retired field-name text; editor round-trip/schema tests remain.
- `DeepSeek1017AndQwenPresetTests.test_requested_inventory_labels_and_fresh_task_defaults` — Duplicates the entire shipped endpoint list/count, making every preset addition fail; focused model wire/default and saved-config tests remain.

## Retained and updated coverage

- Behavioral cache-invalidation coverage in `ApplicationTests.test_endpoint_settings_cache_and_diagnostic_wiring` is retained; only source-string assertions were removed.
- Existing capability-dependent fixtures explicitly call the real `set_chat_feature` API before the first turn. They do not mock the new production defaults or bypass locked permissions. The legacy dynamic checklist-fixture API fallback was simplified to the current public API.
- `ChecklistToggleTests.test_new_chats_default_on_and_one_chats_off_setting_is_not_global` was **renamed/updated**, not deleted: `test_new_chats_default_off_and_existing_permission_is_not_global`.
- `DeepSeek1017AndQwenPresetTests.test_both_qwen_27b_presets_send_xhigh_with_distinct_local_connections` was **renamed/updated**, not deleted: `test_both_qwen_flash_next_presets_send_xhigh_with_distinct_local_connections`.
- Model/reader/subagent tests now assert Flash Next limits and the renamed Novita endpoint; transport-default coverage includes native Messages. Saved/custom configuration persistence tests remain.

## Added coverage

Fifteen new tests cover Anthropic request serialization/authentication; system/image/client-tool mapping; inexpensive utility/compaction effort overrides; signed and redacted thinking replay; real HTTP streaming and non-streaming responses; cumulative cache/token accounting and cache-write costs; a complete chat/tool loop; cancellation and deadlines; dropped/error streams; malformed or empty tool input; stop reasons; credential redaction; fresh-chat permissions and persistence; editable model pricing; and dropdown sizing within viewport bounds.

No paid-provider API calls are made by these tests. Local HTTP fixtures exercise the actual production client and, for the chat/tool case, the actual production chat runtime.

## Final validation (Windows 11, November 7, 2026)

- Python 3.13, pinned PySDL3 0.9.12b1 environment: `python test_velox.py` — **2,463 run; 2,461 passed, 0 failed, 2 skipped**, in 944.476 seconds across 228 isolated class processes.
- Skipped: `ProcessAndOwnershipTests.test_screenshot_suffix_rewrite_cannot_follow_outside_symlink` (Windows symlink privilege not held, WinError 1314); `SettingsAndExecutionTests.test_format_display_datetime_timezone_and_dst` (requires POSIX `time.tzset`). These tests were retained, not removed.
- Python 3.10: all **15 newly added tests passed**; syntax compilation passed on the same final source.
- Production `--init-only` smoke test exited 0 and persisted the requested fresh defaults. Fresh production GUI captures from the final source visibly confirmed disabled optional chat categories, Anthropic Messages connection/pricing, and medium effort.
- `git diff --check` passed. Application release is **326**; supported persisted-data generation remains **316**.
- No live paid-provider calls were made. Anthropic behavior was tested through real local HTTP fixtures, including the complete production chat/tool loop. Linux execution and paid-provider authentication remain untested in this environment.
