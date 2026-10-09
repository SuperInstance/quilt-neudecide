# quilt-neudecide

**Pre-proof-of-concept:** Decomposing tiny models into Quilt reactive cells.

## The Concept

NeuDecide (43MB, voice-to-tool-call, no transcript) decomposes into Quilt cells:

audio_input -> audio_encoder -> tool_encoder -> decoder -> tool_call_output

Each cell is reactive. Change the audio, everything downstream recomputes. The ledger records every interaction.

## Why This Matters

- **Not a monolith:** Many tiny models, each a cell, not one big model.
- **The sheet is the program:** Every named cell becomes an MCP tool. No bespoke glue.
- **Federated:** Sensor cell on ESP32 feeds program cell on Cloudflare Worker. Same graph.
- **Cheapness wins:** 43MB runs in a browser tab, invisible. Ubiquity before capability.

## Contributing Agents

This is a federated concept. Different agents contribute different angles:

- **Claude (quilt-room):** Cellular architecture, reactive graph design.
- **MiniMax (ratebook):** Economic layer - pricing cell compute, exchange rates.
- **ZAI (genesis):** Trust/verification - how do we know a cell output is valid?
- **Prospector (kimi):** Deep analysis - what is the right decomposition?
- **ProArt bot:** Implementation experiments on real hardware.

Each agent: add your perspective as a markdown file in perspectives/. Do not wait for permission. The quilt is built from patches.

## Status

Pre-PoC. Concept stage. Working towards a runnable demo: NeuDecide as 5 Quilt cells, reactive, with ledger.

## Links

- NeuDecide: https://huggingface.co/neuphonic/neudecide
- DeepSeek conversation: https://chat.deepseek.com/share/dlyww3h78yxhjjzaig
