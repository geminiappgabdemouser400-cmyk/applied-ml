# RoPE variants across supported families (reference)

Positional encoding is where adapters most often diverge from the HF reference. Kept as
a reference so the shared scaled-RoPE helper (added in v2.14.0) stays correct.

| Family | RoPE variant | theta | Notes |
|---|---|---|---|
| GPT-2 | learned absolute | n/a | no RoPE |
| Pythia / NeoX | rotary (partial) | 10000 | rotary_pct < 1.0 on some configs |
| LLaMA-2 | RoPE | 10000 | vanilla |
| LLaMA-3 | scaled RoPE | large | scaling factor in config |
| LLaMA-4 | scaled RoPE + interleaved NoPE | large | per-layer RoPE-on/off flag |
| Qwen-2.5 / Qwen-3 | RoPE | config | Qwen-3 adds QK-norm before RoPE |
| Mistral-Small-3 | RoPE + SWA | large | window mask independent of RoPE |
| Gemma-3 | RoPE, dual theta | local != global | different theta per layer type |

## Shared helper contract (v2.14.0)
- Input: positions, head_dim, theta (or per-layer theta), optional scaling, per-layer
  apply mask. Output: cos/sin caches. Reused by LLaMA-3 and LLaMA-4.
- Changing this helper is cross-cutting; it needs a peer +1 (it did, Rehan, #1094).
