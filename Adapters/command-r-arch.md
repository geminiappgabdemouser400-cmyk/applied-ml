# Command-R architecture summary (adapter prep)

Drafted from the public HF `modeling_cohere.py` (CohereForCausalLM / Command-R family).
Basis for the Command-R weight-conversion table (PR #1124, cohere-curious). Logits
match HF to 1e-4 on the smallest public checkpoint. Also tracked by issue #1127
(roadmap: Command-R + DeepSeek).

## Attention
- GQA with separate Q/K/V/O projections (same rearrangement as LLaMA / Qwen2).
  Read `n_key_value_heads` from config; do not infer.
- Parallel attention + MLP: a single `input_layernorm` feeds both attn and MLP.
  There is NO `post_attention_layernorm` (Falcon `num_ln_in_parallel_attn=1` mode).
  Copying a sequential LLaMA block (ln1 → attn → ln2 → MLP) is a structural miss.
- Optional QK-norm is handled inside HF `CohereAttention.forward`; no extra TL
  tensors to map for the first adapter.

## Positional encoding
- Interleaved RoPE via `CohereRotaryEmbedding` (delegated to the HF module).
  Uses `repeat_interleave` rather than a cat-split; do not re-implement in TL.
- `rope_theta` from `config.rope_parameters` (fallback `default_theta`, usually 10000).

## Norm / MLP
- True LayerNorm (`CohereLayerNorm`): mean-subtracting, weight but NO bias.
  Not RMSNorm. `uses_rms_norm=False`. Treating it as RMSNorm is a silent logit error.
- SwiGLU MLP (`gate_proj` / `up_proj` / `down_proj`). No projection biases
  (`attention_bias=False`).

## Embeddings / logits
- Tied embed/unembed by default (`tie_word_embeddings=True`). Keep W_E / W_U aliased
  unless a checkpoint unties them.
- Output logits multiplied by `config.logit_scale` (default 0.0625 = 1/16). Fold into
  `unembed.weight` in `preprocess_weights` after cloning so the tied embedding row is
  not scaled. Dropping this scale fails the 1e-4 test.

## Tokenizer
- `CohereTokenizerFast.add_bos_token` is False, but HF `__call__` with
  `add_special_tokens=True` (the default) still prepends BOS. Set
  `default_prepend_bos=True`. Verified on `trl-internal-testing/tiny-CohereForCausalLM`.

## Quirks / gotchas
- Parallel residual + single LN is the first easy trap.
- LayerNorm vs RMSNorm (mean-subtract) is the second.
- `logit_scale` fold must not mutate the tied embedding.
- `logit_scale` is not a declared TransformerBridgeConfig field; pass it through or
  the adapter falls back to 0.0625 even when the checkpoint differs.

## Acceptance
- float32 logits to 1e-4 absolute on the smallest CohereForCausalLM checkpoint.
  PR #1124 reports a pass on that checkpoint.
