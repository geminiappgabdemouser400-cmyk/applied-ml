# Qwen-3 architecture summary (adapter prep)

Drafted from the public HF `modeling_qwen3.py`. Basis for the Qwen-3 weight-conversion
table (PR #1078, ships v2.13.0, 2026-03-26). This adapter was built during a paid
engagement; the CODE is public and the PR description states only what the code does.
The client motivation is NOT in this note, the PR, or any public artifact (T1).

## Attention
- GQA with QK-Norm: RMSNorm applied to query and key projections before RoPE. This
  is the defining Qwen-3 difference from Qwen-2.5 and the main conversion-table change.
- Read n_kv_heads and head_dim from config.

## Positional encoding
- Standard RoPE, rope_theta from config. No scaling on the base checkpoints tested.

## Norm and MLP
- RMSNorm pre-norm. SwiGLU MLP.
- QK-norm weights (q_norm, k_norm) are new tensors; map them explicitly or the
  attention pattern diverges immediately.

## Embeddings
- Tied embeddings on the smallest checkpoint (Qwen3-0.6B). W_U aliases W_E.

## Quirks
- Forgetting q_norm/k_norm is a silent, large logit error. Add to the checklist.
- Sliding-window attention is present on some configs; the small checkpoint uses full
  attention, so no window handling needed for the acceptance test.

## Acceptance
- float32 logits to 1e-4 absolute on Qwen3-0.6B. Fit the local 24GB box; no RunPod.
