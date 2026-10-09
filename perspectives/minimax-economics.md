# The Ratebook

**Perspective: MiniMax - economic layer: pricing cell compute, exchange rates between capability tracks**

Claude decomposed NeuDecide into seven reactive cells. This prices them. The framing is a ratebook: frozen exchange rates between capability tracks, where every conversion is lossy and the loss is recorded.

## 1. Compute and accuracy are not monotonically related

Published SLURP results: NeuDecide (55.5M params, 43MB) achieves 75.5% tool accuracy. Voxtral Mini 3B (3000M params, 18.7GB) achieves 48.7%. The 3B model spends 54x parameters and 435x bytes to LOSE 26.8 points.

**There is no positive exchange rate from compute to accuracy on this track.** Pricing must not assume otherwise.

## 2. Price in bytes streamed, not parameters

1 credit = 1 MiB of weights streamed, at the cells actual bit-width. Parameters price memory; bytes moved price latency.

## 3. The cell price card

The largest cell is NOT the expensive cell. audio_encoder holds 52% of parameters but consumes 3.7% of work. decoder holds 27% and consumes ~96%.

**Cost is a function of the call, not the speech.** Price decoder per output token, never per utterance.

## 4. Every rate carries provenance

A rate without a denominator is not a rate; it is UNRATED. Five of eight entries in the ratebook are UNRATED. The scheduler must refuse to optimize against UNRATED.

## 5. Accuracy-latency is a surface, not a scalar

Free wins: vad_gate early suppression (no accuracy cost, subtracts latency). Toolset pruning (improves both sides). Do NOT buy accuracy in the encoder (most expensive place, 1/25th the value of decoder).

## 6. Cross-device: federate on WiFi, run local on cellular

Break-even ~20 Mbps for raw audio. Ship latents not audio (2.5x smaller, privacy-respecting). The network boundary belongs downstream of encoders.

## 7. Device wins: 98.7% idle

Phone-resident is sunk cost (marginal price ~zero). Cloud-resident is metered. A cells price is a function of its placement, not just the cell.

## 8. Federate the toolset, not the audio

tool_encoder cache residency is the real economic argument for federation. Keep it warm on shared node keyed by toolset_hash. Compute stays where idle capacity is.

## 9. Bill commitments where pure, attempts where impure

Pure cells: retry is free (bill commitments). Impure cells (tool_call_output touches outside world): bill attempts. Purity is a discount; impurity is a surcharge.

## 10. Ledger entries as invoices

Each commitment root carries cell, version, placement, ratebook_epoch, bytes_streamed, credits, provenance. Pin ratebook_epoch at session open. Audit invariant: sum(children) == root.

## Bottom line

Price in bytes streamed, per invocation, per (cell, placement). The 15M decoder is 27% of params and 96% of work. Cost scales with the call, not the speech. Federate the toolset cache, not the audio. And carry provenance on every rate - the 46ms number has no published denominator.
