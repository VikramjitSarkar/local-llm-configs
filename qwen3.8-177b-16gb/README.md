# Qwen 3.8 Flash Next 177B on a 16 GB GPU (llama.cpp)

**Hardware:** RTX 4080 SUPER 16 GB, 32 GB DDR5, Core Ultra 7 265KF, Samsung 980 PRO NVMe, Windows 11
**Model:** `ISTA-DASLab/Qwen3.8-Flash-Next-GSQ-RCO-GGUF`, IQ3_XXS (2 files, 75.8 GB). 512 experts, 10 active per token.
**Engine:** plain llama.cpp b11900 (CUDA). No Ollama, LM Studio or Strata.

## Command
```
llama-server -m <model>.gguf -c 65536 -np 1 --jinja -ngl 99 --fit off -fa on ^
  -ctk q8_0 -ctv q8_0 --load-mode mmap --cache-ram 0 -b 2048 -ub 2048 ^
  --cache-reuse 256 -ncmoe 42 -t 12
```

## What the important flags do
- `-ngl 99`: put every layer on the GPU…
- `-ncmoe 42`: …except the **expert** weights of 42 layers, which stay in RAM/SSD. Only 10 of 512 experts work per token, so the slow ones are rarely touched. Less VRAM → raise N.
- `--load-mode mmap`: read the 76 GB file straight from the SSD instead of copying it into RAM.
- `-ctk/-ctv q8_0`: 8-bit KV cache, half the memory for the conversation.
- `--cache-reuse 256`: reuse the cached prompt when an agent re-sends almost the same context.

## Numbers
| Setting | tok/s | VRAM |
|---|---|---|
| `--cpu-moe` (all experts on CPU) | 11.2 | low |
| `-ncmoe 42` | 15.2 (avg 18.2, best 22.6 in chat) | ~13.2 GB |

- Prompt processing ~32.7 tok/s. GPU only ~24% busy: the bottleneck is RAM/SSD, not the card.
- 2 h 30 min OpenCode run, 39 calls, RAM 31.4 / 31.6 GB, no crash.

## Tips
- Reboot before benchmarking. After a day of uptime speeds can drop a lot; a reboot fixes it.
- Close the browser before loading on 32 GB RAM.
