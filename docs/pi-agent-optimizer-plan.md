# Pi Agent Local-Model Optimizer — claws-mac-mini

**Use Fable 5 (access ends July 7) to select and tune the best local models for claws-mac-mini's hardware and the Pi Agent harness — reachable over Tailscale SSH, wired into Hermes and the Organized Fable local tier.**

Inspired by Sam Gaddis's setup: Hermes on a 16GB mac mini + Codex sub, Gemma for nightly hygiene, qwen3-coder on the big box — and the key move: *tell Fable to find the best local model for the specific hardware + task, then `/goal` to optimize it to the hilt.*

- **Target:** `claws-mac-mini` · Tailscale `100.82.244.127` (16GB M-series; runs Hermes + OpenClaw + local render)
- **Control machine:** supabowl (M1 Pro) -> Tailscale SSH -> claws-mac-mini
- **Design stack:** reuse the metaharness HTML stack (IBM Plex Mono, dark + lime, GSAP)
- **Ties into:** Organized Fable (local = local tier), Hermes routing, PI Agent genetic routing
- **Storage (live):** fable-cal Worker D1 `fable-ledger` -> tables `pi_models`, `pi_tasks` (task taxonomy seeded)

## The strategic constraint (July 7)

Fable access ends tomorrow. The plan splits hard:

- **FABLE-CRITICAL (do first, today):** model selection per task + the `/goal` optimization recipes. Capture every Fable output as a durable artifact so the intelligence survives the cutoff.
- **POST-CUTOFF (no Fable needed):** benchmarking, wiring, automation, dashboard data — fine on Opus/Sonnet/local after July 7.

## Phased implementation (order only)

- **Phase 0 - Access + deploy:** confirm Tailscale SSH to claws-mac-mini; deploy the optimizer surface (Worker Assets + GSAP) on the fable-cal project; `pi_models`/`pi_tasks` D1 tables (done).
- **Phase 1 - Inventory:** SSH-profile claws (RAM/Metal/thermal/disk/runtimes); enumerate the Pi Agent task taxonomy (hygiene, coding, routing, embeddings) with SLAs.
- **Phase 2 - [FABLE] Model selection:** best local model per task within 16GB (family, size, quant q4_K_M, framework mlx/Ollama/llama.cpp). Emit durable `model-selection.md`.
- **Phase 3 - [FABLE] `/goal` optimization:** per model, tune quant, context, KV cache, Metal flags, speculative decoding, templates. Capture `recipes/<model>.yaml` + `playbook.md`.
- **Phase 4 - Benchmark harness:** tok/s, task-eval quality, mem headroom, thermal over Tailscale SSH -> D1.
- **Phase 5 - Pi Agent + Hermes wiring:** route optimized models via genetic-algorithm routing; register as Organized Fable's local tier + 3rd inference source (Anthropic OAuth / OpenAI OAuth / PI local).
- **Phase 6 - Nightly hygiene:** gemma-class LaunchAgent on claws (digest/cleanup) -> Fable ledger.
- **Phase 7 - Dashboard live data + GSAP polish.**

## Task taxonomy (seeded in D1 `pi_tasks`)

| task | quality bar | notes |
|---|---|---|
| nightly-hygiene | 0.6 | gemma-class digest/cleanup/log-triage |
| coding | 0.85 | qwen3-coder q4_K_M within 16GB |
| routing-classify | 0.7 | tiny model — Organized Fable scoring agent |
| embeddings-rag | 0.75 | Penumbra knowledge embeddings |

## The one thing that matters most

By end of day July 6, have `model-selection.md` + `recipes/*.yaml` + `playbook.md` written by Fable. Everything after is executable without it.
