# HookedTransformer: architecture overview (maintainer note)

For onboarding and for keeping adapter authors oriented. HookedTransformer is the core
class: it loads a pretrained model into a uniform architecture with hooks at every
residual-stream, attention-head, and MLP position, plus ActivationCache for capturing
and editing those activations. Analysis-first, NOT a training framework (T4: decline
training-loop requests, point to TRL).

## Load path
1. `from_pretrained(name)` resolves the family via the model-adapter registry.
2. `loading_from_pretrained.py` maps HF config -> TL config and HF weights -> TL weights
   (the weight-conversion table; Bram's area).
3. The model is instantiated with hooks wired at every standard point.

## Hook points (the stable surface users depend on)
- `hook_embed`, `hook_pos_embed`
- per block: `hook_resid_pre`, `attn.hook_q/k/v/z`, `hook_attn_out`, `mlp.hook_pre/post`,
  `hook_mlp_out`, `hook_resid_mid`, `hook_resid_post`
- `ln_final.hook_normalized`, `hook_logits`

## ActivationCache
- `run_with_cache` captures a dict of activations keyed by hook name. SAEs and patching
  workflows depend on the exact key names and shapes; a reshape change (e.g. hook_z
  under GQA) is a breaking edge case (see #1069) and any signature change is cross-cutting.

## Invariants adapters must preserve
- Hook names and shapes are the public contract. New families reuse the same names.
- float32 logits match HF to 1e-4 absolute on the smallest public checkpoint.
