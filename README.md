# local-llm-configs

Exact settings I use to run big local AI models on normal hardware, with the real numbers I measured. Every config here is shown working on my YouTube channel, [IamVikramjit](https://youtube.com/@IamVikramjit).

| Folder | What | Hardware | Result |
|---|---|---|---|
| [qwen3.8-177b-16gb](qwen3.8-177b-16gb) | Qwen 3.8 Flash Next 177B (76 GB file) on a 16 GB GPU with llama.cpp | RTX 4080 SUPER 16 GB + 32 GB RAM | 15–18 tok/s, 2.5 h agent run, no crash |
| [inkling-2-mini](inkling-2-mini) | Inkling 2 Mini, GGUF vs MLX on a Mac, and the 16 GB PC tweak | MacBook Pro M5 32 GB / RTX 4080 SUPER | Mac 21–22 tok/s, PC 8 → 38 tok/s |
| [opencode](opencode) | OpenCode config for LM Studio + oMLX (local coding agent) | any | |

Videos:
- 177B on 16 GB VRAM: https://youtu.be/5zDrZ5MRZgM
- Free local AI coding on a Mac (configs in [free-local-ai-coding-mac](https://github.com/VikramjitSarkar/free-local-ai-coding-mac))

Questions or your own numbers: Discord https://discord.gg/C2HHMeweh9
