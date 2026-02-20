# Changelog

## [0.2.0] — 2026-02-20 (Fork: coolmanns/openclaw-plugin-continuity)

### Added
- **llama.cpp GPU embedding backend** — Primary embedding via HTTP to a llama.cpp server running `nomic-embed-text-v1.5` (768d). 60x faster indexing, 20x faster search vs ONNX on CPU.
- **ONNX fallback** — If llama.cpp is unavailable, falls back to the original `@chroma-core/default-embed` (384d) seamlessly.
- **Configurable endpoint** — Set `LLAMA_EMBED_URL` env var to point to your llama.cpp server (default: `http://localhost:8082`).
- **Auto dimension detection** — Embedding dimensions are set dynamically based on which backend initializes. Vector table is recreated automatically if dimensions change.
- **Search telemetry** — Every search logs to `/tmp/openclaw/memory-telemetry.jsonl` with system, query, latency, distances, result count, and injection status.
- **QMD/BM25 telemetry** — `tool_result_persist` hook logs built-in memory_search results for cross-system comparison.

### Fixed
- **Init order bug** — `_createTables()` was running before `_initEmbeddings()`, creating the vector table with default dimensions (384) before llama.cpp could set it to 768. Tables now created after embeddings initialize.
- **Log levels** — Search status messages (`[Continuity] Search:`, result counts, scores) changed from `console.error` to `console.log`. Only actual failures remain as errors.
- **Package name mismatch** — Renamed `package.json` `name` from `openclaw-plugin-continuity` to `continuity` to match the manifest `id`, eliminating the config mismatch warning on every gateway start.

### Changed
- Default embedding dimensions changed from 384 to 768 (matches nomic-embed-text-v1.5 output).
- `search_document:` prefix added automatically for indexing, `search_query:` for retrieval (required by nomic-embed-text).

## [0.1.0] — 2026-02-15 (Upstream: CoderofTheWest/openclaw-plugin-continuity)

Initial release by Chris Hunt (CoderofTheWest).

- Cross-session conversation archive with SQLite + sqlite-vec
- Semantic search with ONNX embeddings (all-MiniLM-L6-v2, 384d)
- Topic tracking with fixation detection
- Continuity anchors (identity, contradiction, tension)
- Context budgeting with priority tiers
- Temporal re-ranking (recency boost)
- memory_search enrichment via tool_result_persist
- Proprioceptive framing ("You remember..." injection)
