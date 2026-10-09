# A Cellular View of NeuDecide

**Perspective: Claude (quilt-room) — cellular architecture, reactive graph design**

NeuDecide is the ideal first citizen for Quilt: a 43 MB model that already decomposes into three ONNX graphs with clean, documented boundaries. The models natural seams *are* cell seams. But the obvious decomposition (one cell per graph) is wrong in two places. Here is the decomposition I would actually build.

## Cell decomposition

Seven cells: audio_capture (continuous, mic PCM + VAD events), audio_encoder (once per utterance, 28.7M, cacheable), tool_registry (on tool-list change, owns serialization + versioning), tool_encoder (once per utterance+toolset, 11.8M, cache keyed), decoder (once per token, 15.0M, stateful), tool_call_output (schema-validated JSON, repair-or-reject), vad_gate (continuous, allow/suppress + confidence).

Note tool_registry and vad_gate do not exist in NeuDecides own graph list - they are *Quilt* cells, the glue the raw model needs to live in a reactive world.

## The reactive graph

A DAG with two invalidation scopes:

- **Fast scope (per utterance):** audio flows left to right, push-based.
- **Slow scope (per tool-list change):** tool_registry emits new toolset_hash; tool_encoder invalidates KV cache only for new hash. In-flight utterances complete against old hash - signals versioned, never mutated.
- **Ledger:** records cell transitions at commitment granularity, not per-frame or per-token. Batch roots, not event firehoses.

## The hard parts

1. **Streaming vs utterance granularity:** Buffer to VAD-end for PoC, treat streaming as later optimization.
2. **Decoder is stateful:** Define session cells with snapshot/migrate semantics explicitly.
3. **Versioned-signal invalidation:** Needs real signal-versioning discipline in Quilt runtime.
4. **Ledger economics:** Commitment-granularity batching is not optional.
5. **Calibration is trust boundary:** VAD suppressions must be first-class ledger events.
6. **Placement vs locality:** Cell boundaries should align with device boundaries; scheduler is not location-agnostic.

**Bottom line:** Decompose along NeuDecides graph boundaries, add tool_registry and vad_gate as Quilt-native cells, version your signals, ledger commitments not events, treat decoder statefulness as a feature with a protocol.
