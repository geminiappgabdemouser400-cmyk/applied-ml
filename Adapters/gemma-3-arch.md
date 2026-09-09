# Gemma-3 architecture summary

Basis for PR #1112 (Bram-authored, ships v2.16.0, 2026-06-25). NOTE: shipped with a
norm-epsilon placement bug in the conversion table; caught inside the 48h monitoring
window (issue #1121) and fixed in the v2.16.1 hotfix (2026-06-27). See
`Notes/triage_1121_gemma3_logits.md`.

## Attention
- GQA. Interleaved local (sliding-window) and global attention layers, alternating on
  a fixed period read from config. Getting the per-layer local/global flag wrong is a
  logit mismatch; it did not cause the v2.16.0 miss but is the other easy trap.

## Positional encoding
- RoPE with different theta for local vs global layers (two rope_theta values). The
  conversion table selects theta per layer type.

## Norm / MLP (the bug)
- RMSNorm with a query/key norm AND a post-attention/post-MLP norm (extra norms vs
  LLaMA). Gemma applies norm as (1 + weight) * x, and the eps is applied inside the
  rsqrt. The v2.16.0 table placed eps outside, producing a ~1.3e-4 max logit delta on
  the smallest checkpoint, over the 1e-4 threshold. v2.16.1 moves eps inside the rsqrt
  to match HF; delta back to ~2.9e-5.
- Logit soft-capping (as in Gemma-2) may be present on some configs; map the cap value.

## Embeddings
- Tied embeddings; embedding scaled by sqrt(d_model) at input (Gemma-specific). Do not
  drop the input scaling.

## Acceptance
- v2.16.0: FAILED threshold post-release (1.3e-4). v2.16.1: PASS (2.9e-5).
