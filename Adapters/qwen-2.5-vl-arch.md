# Qwen-2.5-VL architecture summary (text-stack adapter)

One-time paid engagement for Dr. Nkechi Osunde-Ferrar ahead of a paper deadline
(invoice HB-2026-021, $870, 6 hours). Adapter landed upstream as PR #1066 (v2.12.0,
2026-02-26). The vision tower is OUT OF SCOPE for TransformerLens; we adapt the text
decoder stack only and document that clearly.

## Scope
- Text decoder only. The vision encoder + merger are not hooked; loading a VL checkpoint
  into HookedTransformer exposes the language-model activations only.

## Attention / norm / MLP
- GQA, RMSNorm pre-norm, SwiGLU. Same base as Qwen-2.5.
- M-RoPE (multimodal RoPE) collapses to standard RoPE when there are no image tokens;
  the text-only adapter uses the text RoPE section. Documented as a known limitation:
  interleaved image position ids are not modeled.

## Embeddings
- Tied on the 3B checkpoint tested.

## Acceptance
- float32 logits to 1e-4 absolute on a text-only prompt, Qwen2.5-VL-3B text stack.
- Documented limitation: image-conditioned positions unsupported (text stack only).
