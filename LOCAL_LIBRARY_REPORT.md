# Local Library — v337 implementation and validation

Implemented in the chat-local checkout of `https://github.com/triplenom/velox`, based on `af4ba73` (v336). The change is one local commit; no push was performed.

## What changed

- Added **Local Library**, on by default, to **NEW CHAT SETTINGS**. It is saved per chat, retained by copies/branches, inherited/narrowed by agents, and locked with existing tool choices at the first user message. It is independent of File and System. Disabled library tools are omitted from native schemas and rejected at dispatch, including agent calls.
- Added shared `library/` beside `velox.py`, independent of working/data directories. Startup creates it if absent. User documents, subfolders and `.cache/` are git-ignored; the explanatory `library/README.md` is tracked.
- Added unindexed, streaming UTF-8 text/Markdown scanning with `pathlib`, `os`, `re` and other standard-library facilities. Literal case-insensitive keywords and quoted phrases use deterministic phrase/co-location ranking, short excerpts, original relative paths and inclusive 1-based line ranges. Overlapping excerpts are deduplicated. Search retains bounded fragments and top results, including for giant single lines.
- Reused `AttachmentStore._convert_to_markdown` through an opt-in `local_only` branch, leaving existing conversion callers unchanged. PDF/DOCX/PPTX/XLSX/XLS Markdown is cached per original path and source stat signature under `library/.cache/`. Unchanged sources reuse the cache; changed sources reconvert. A source changed during conversion is reported, not committed as a valid cache entry.
- Conversion runs in a killable Python child; plugins, LLM clients, cloud endpoints and OCR are not enabled, and Python socket connections are refused in that child. No search executable, database, index, embeddings, RAG framework or LLM retrieval stage was added. The existing converter's optional dependencies are pinned at the package level in `requirements-library.txt`: `markitdown[pdf,docx,pptx,xlsx,xls]==0.1.5`.
- Search and reads run off the UI thread. Search timeout/cancellation/file errors explicitly set `incomplete` with reasons and bounded diagnostics. General tool cancellation also marks an interrupted search incomplete. Result caps have a separate `results_limited` flag.
- Library path handling rejects outside paths, traversal, alternate data streams, symlinks/junctions/reparse points and direct source access to `.cache/`. Source documents are never written; only derived cache files are created.
- Kept system-prompt growth below the existing **13,500-character** regression limit by placing detailed API guidance in native tool descriptions rather than expanding the frozen prefix. No test budget was relaxed.
- Kept PC Control and Local Library readable on narrow settings cards by giving them separate full-width rows when they cannot fit side by side.
- Incremented `CURRENT_VERSION` from 336 to **337**. Persisted data generation remains **316**, and config version remains **1**. Older compatible records may omit the new flag (default on); other stored tool choices and existing configuration files are preserved. Legacy Computer Use still defaults off.

## Added native tools

| Tool | Contract |
| --- | --- |
| `library_search(query, max_results, timeout_in_seconds)` | 1–50 results; 0.1–120 seconds; up to 32,000 serialized result characters. Original source paths, line ranges, excerpts, scores, completeness and cap/error metadata. |
| `library_read_doc_lines(path, start_line, end_line)` | Original library-relative or contained absolute path; inclusive 1-based lines; at most 1,000 requested lines and 24,000 returned characters. |
| `library_read_doc(path)` | Original source path; up to 24,000 returned characters with explicit truncation. |

Reads resolve cached conversions transparently. PDF/Office line numbers describe converted Markdown, not page numbers. Read operations have a 120-second deadline. A read ending exactly at its character cap does not count lookahead text as a returned line.

## Validation results

Environment: Windows 11 Pro, Python **3.13.5**, PySDL3 **0.9.12b1**, Pillow **12.3.0**, MarkItDown **0.1.5** with the listed local format extras. The final full run used a chat-owned TEMP/TMP directory.

- **Full existing + added suite:** `python test_velox.py` — **2,622 tests**, **2,616 passed**, **6 skipped**, **0 failures/errors**, **246 isolated classes**, **1,146.176 seconds**, exit **0**. The native runner validated every class result/count.
- **New LocalLibraryTests:** **25 tests**, **24 passed**, **1 skipped** (file-symlink creation privilege). These are included in the full totals above.
- **Focused final regressions:** `python test_velox.py --test-class LocalLibraryTests,RunnerPortabilityRegressionTests,StallAndProgressTests,ToolPolicyAndImageExecutionTests` — **65 tests**, **64 passed**, **1 skipped**, exit **0**.
- Real offline PDF, DOCX, PPTX and XLSX fixtures converted, searched and read successfully; subsequent reads reused the cache without conversion.
- Production SDL frames at 1920×1080 were inspected with Local Library on and after a real widget-path click turned it off; persistence was read back. Separate narrow/wide settings-card renders verified the responsive rows. These are runtime renders, not HTML or mock screenshots.
- `velox.py --init-only --config <isolated config>` passed from a separate data directory.
- `python -m py_compile velox.py test_velox.py` and `git diff --check` passed.

The final source and test hashes were recorded and matched after the complete run:

```
velox.py       f8bc0a56513e48ab5bad285be5b78cf347225d96f1329d5dd562e1bf8d28cec4
test_velox.py  91ad7f4d0f61363afb806e682be7cace1673006e6d58444ebc879dfc9ae57c3a
```

The full console log and native-rendered screenshots are retained in the surrounding chat workspace, not committed as application data.

### All added test methods

- `test_keywords_ranking_phrases_and_original_line_ranges`
- `test_phrase_excerpt_is_relevant_after_long_whitespace_and_chunk_boundaries`
- `test_unicode_casefold_bom_subfolders_and_cross_line_phrase`
- `test_deduplicates_overlaps_and_caps_output_deterministically`
- `test_only_supported_files_and_no_cache_search`
- `test_bounded_reads_inclusive_lines_and_empty_ranges`
- `test_exact_read_cap_does_not_count_lookahead_as_a_returned_line`
- `test_giant_lines_stream_fully_and_invalid_utf8_is_explicit`
- `test_conversion_cache_invalidates_and_reads_keep_original_source`
- `test_conversion_failure_and_source_change_are_not_silent`
- `test_traversal_absolute_outside_ads_and_cache_are_rejected`
- `test_symlink_and_windows_junction_escapes_are_rejected`
- `test_file_symlink_is_rejected_without_following_it`
- `test_cache_junction_cannot_write_outside_library`
- `test_timeout_cancel_and_async_worker_not_ui_thread`
- `test_killable_converter_cleans_up_on_deadline_and_cancellation`
- `test_disabled_permissions_system_independence_and_agent_inheritance`
- `test_copy_branch_and_child_agents_preserve_or_narrow_library_permissions`
- `test_legacy_records_preserve_settings_and_default_library_on`
- `test_query_and_numeric_validation`
- `test_real_offline_pdf_docx_pptx_xlsx_conversion_and_cache_reuse`
- `test_converter_local_only_options_never_enable_plugins_or_llm`
- `test_library_toggle_real_widget_click_persists_and_locks`
- `test_narrow_settings_preserve_full_library_and_pc_control_labels`
- `test_registry_timeout_and_user_cancel_report_incomplete`

## Limitations and retained pre-existing issues

The six skipped cases were:

1. Opt-in real Windows desktop mouse/keyboard automation (not enabled).
2. Fractional-offset timezone control, unavailable on Windows.
3. Toronto DST timezone control, unavailable on Windows.
4. New file-symlink containment integration case: Windows refused symlink creation with error 1314. **Actual directory-junction containment and cache-junction write-escape tests did run and pass.**
5. Existing screenshot-output symlink test: the same Windows privilege limitation.
6. Existing display-time/DST test requiring POSIX `time.tzset`.

Linux was not run. Legacy XLS uses the existing MarkItDown converter and installed XLS extra, but no real XLS fixture was added; real format fixtures covered PDF/DOCX/PPTX/XLSX. Image-only PDFs do not gain OCR text.

At an intentionally narrow 1280×720 layout with a large sidebar and short composer, the existing composer summary can spill upward and other old category labels may truncate. The same summary overflow and old-label truncation were reproduced using the unmodified v336 baseline. The new Local Library/PC Control card-row clipping was fixed; the unrelated existing composer layout was left outside this narrow feature change.

Library scanning/conversion is local, but returned text can be sent to the configured model endpoint. This is not an OS sandbox for separately enabled general File/System tools.
