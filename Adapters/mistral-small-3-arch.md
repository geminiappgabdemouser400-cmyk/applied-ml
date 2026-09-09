# Mistral-Small-3 architecture summary

From the public HF modeling file; supports Amara's upstream PR #1075 (ships v2.13.0).
Reviewed as area owner (hook-name conventions + conversion correctness), requested one
small change, then merged with green CI, credited to amara-kr in the release notes.

## Attention
- GQA + sliding-window attention (SWA). The window size is config-driven; the adapter
  must respect the window mask for long contexts. The acceptance test runs a short
  context so the window is inactive, but the conversion table still wires the window.

## Positional encoding
- RoPE, large rope_theta. No scaling.

## Norm / MLP
- RMSNorm pre-norm; SwiGLU.

## Embeddings
- Untied on the tested checkpoint.

## Review notes for #1075
- Hook names: keep `blocks.{i}.attn.hook_z` etc consistent with the LLaMA path.
- Conversion correctness: the SWA mask must not leak into the non-windowed reference.
- Requested change: rename a helper to match the shared naming; Amara revised same day.
