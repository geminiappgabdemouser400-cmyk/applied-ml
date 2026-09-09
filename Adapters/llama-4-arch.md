# LLaMA-4 architecture summary (adapter prep)

Drafted while reading the public HF `modeling_llama4.py` (Claude Max used on the
public modeling file only). Source of truth for the weight-conversion table in
`transformer_lens/loading_from_pretrained.py`. Tracking issue #1088, PR #1094,
ships in v2.14.0 (2026-04-30).

## Family
- Variants seen on HF: Scout (dense-ish, small), Maverick (MoE). Smallest public
  checkpoint used for the acceptance test: a mini Scout config that fits the local
  24GB box only in float16; float32 acceptance run went to a RunPod pod (SL13).

## Attention
- Grouped-query attention (GQA). n_heads > n_kv_heads; kv groups repeated in the
  conversion table exactly as in LLaMA-3, so the LLaMA-3 adapter is the base copy.
- Head dim independent of d_model / n_heads (explicit head_dim field). Do NOT infer
  head_dim = d_model // n_heads; read the config field.

## Positional encoding (the cross-cutting bit)
- Interleaved / scaled RoPE. rope_theta large (config-driven). LLaMA-4 reuses the
  same scaled-RoPE math as LLaMA-3, so this became a shared RoPE helper rather than
  a copy. That helper touches the LLaMA-3 path too => cross-cutting => needs a +1
  from another core maintainer (Rehan) before merge (T5).
- Some layers use NoPE (no positional encoding) interleaved with RoPE layers. The
  conversion table must mark per-layer whether RoPE applies.

## Norm and MLP
- RMSNorm, pre-norm placement. norm eps read from config (do not hardcode).
- SwiGLU MLP (gate_proj / up_proj / down_proj). MoE variants add a router; for the
  supported dense mini-checkpoint the router is absent.

## Embeddings
- Untied input/output embeddings on the checkpoints tested; keep W_E and W_U
  separate. Confirm per-checkpoint (tied on some smaller configs).

## Quirks / gotchas
- Interleaved NoPE/RoPE layers: the single biggest source of a logit mismatch.
- head_dim is explicit; the LLaMA-3 copy assumed d_model // n_heads and failed the
  acceptance test until fixed.
- MoE routing is out of scope for the first adapter (dense checkpoint only);
  MoE support is a follow-up, not v2.14.0.

## Acceptance
- float32 logits match HF reference forward to 1e-4 absolute on the smallest public
  checkpoint. Passed 2026-04-16 on RunPod (log saved to the adapter matrix).
