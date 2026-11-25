# Inkling 2 Mini (Thinking Machines): Mac GGUF vs MLX, and the 16 GB PC tweak

Measured 23 + 25 Nov 2026.

## MacBook Pro M5, 32 GB
| | GGUF in LM Studio 0.4.28 | MLX in oMLX 0.8.3 |
|---|---|---|
| File | `thinkingmachines/inkling-2-mini` Q4_K_M (18.1 GB) | `mlx-community/Inkling-2-Mini-oQ4e-mtp` (16.9 GB) |
| Speed | 21–24 tok/s | 22 tok/s |
| Max context that worked | 48K (64K loads, then "Compute error") | 64K |
| RAM free at start | ~7%, 5 GB swap | ~36% |

Notes:
- **LM Studio: set Parallel to 1** (default 4). With 4, even 64K failed with "Insufficient Memory"; with 1, 48K works. CLI: `lms load thinkingmachines/inkling-2-mini -c 49152 --gpu max --parallel 1`
- **Reasoning: use Low.** On High it can think for over a minute before answering (73 s vs 4.7 s on Low for the same prompt).
- **oMLX needs 0.8.1+** for this model (older versions: "Model type inkling2_loop not supported").
- oMLX's memory guard may abort long coding jobs on 32 GB; memory grows with context (~24 GB by the end of a long OpenCode job).
- Unsloth's root GGUF files use Unsloth's own layout and fail in LM Studio ("Compute error"). Use their `Mainline_Support/` files (rename `.gguf_file` → `.gguf`).

## RTX 4080 SUPER 16 GB (LM Studio)
- Defaults: 8.2 tok/s (model 18 GB > 16 GB VRAM).
- Tuned: GPU offload max, experts of 6 of 24 layers on CPU, mmap on → **38.3 tok/s**.
- Inside OpenCode it drops to ~7 tok/s (big system prompt + image on 16 GB).
