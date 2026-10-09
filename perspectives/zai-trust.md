# The Genesis Anchor, Applied: Trust and Verification for Quilt Cells

**Perspective: ZAI - trust and verification. Applying THE GENESIS ANCHOR to the quilt-neudecide cellular decomposition.**

**Thesis:** In a federated Quilt runtime, cells are mutually untrusted. Nothing checks a cell except what is frozen. Cells propose; the anchor disposes. No cell judges another - ever.

## 1. How is a cell output verified?

Every cell output arrives with a verification receipt: (input_hash, params_digest, output_hash, cell_model_digest, nonce) keyed to a public per-epoch randomness beacon. Three frozen checks:

- **Execution track:** Deterministic cells (audio_encoder, tool_encoder) verified by replay. Spot-checks sampled by beacon. Weight digests hash-committed at genesis.
- **Proof track:** Non-replayable outputs (decoder tool calls) checked against pinned conformance kernel - grammar, bounds, required fields. No learned judgement, only contract conformance.
- **Prediction track:** Probabilistic outputs scored against pre-committed held-out corpora with timelock-revealed labels.

## 2. vad_gate suppressions: who audits them?

The frozen court, not the cell itself. Each suppression is a first-class ledger event: (frame_window_hash, decision, vad_model_digest, calibration_certificate).

- **Per-event:** Court recomputes on sampled windows with pinned VAD binary.
- **Statistical:** Scored on genesis-committed held-out corpus. False-suppression rate is a frozen bound - silently dropping user speech is a safety property.
- **Rate:** Ledger batch roots make suppression rates mechanically checkable per epoch.

If calibration drifts past frozen bound, court stops accepting suppressions. Fail-open to user, fail-closed to adversary.

## 3. Trust boundary between cells

Not between cells - between each cell and the anchor. Cells sit in untrusted zone; only trusted surface is frozen court. Downstream consumes upstream output only if receipt verifies. Receipt issued by court, never by producing cell.

Ledger stores receipts, not data. tool_registry toolset_hash is anchor-committed: new hash invalidates decoder KV cache AND re-pins conformance kernel schema registry.

## 4. Federated devices: remote cell lying?

We do not trust the device. We trust the receipt.

- **Challenge-response:** Beacon nonces, single-use, non-transferable.
- **Pinned digests:** Genesis includes weight digests. Lying detected probabilistically per output, certainly over time.
- **zkVM proofs:** Where replay impossible, succinct proof of correct execution against pinned digest.
- **Ratchet:** Trust only tightens. Certified error bound monotone non-decreasing. Cannot quietly get worse.

**Boundary claim:** Anchor does not claim frozen checks cover everything - only that everything acceptance-critical must be frozen, fresh, mechanically checkable.
