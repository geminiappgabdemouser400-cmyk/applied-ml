# Model-adapter subsystem design (loading_from_pretrained.py)

Bram's primary owned area. This note is the mental model for how a HF model becomes a
HookedTransformer.

## Registry
- A family is registered with: canonical name(s) / HF aliases, a config translator, a
  weight-conversion function, and per-family flags (GQA, tied embeddings, RoPE variant,
  norm placement, embedding scaling, local/global attention schedule).

## Config translation
- HF config -> TL `HookedTransformerConfig`. Read fields, never infer (head_dim is the
  canonical trap). Unknown/optional fields default conservatively and are logged.

## Weight conversion
- HF state_dict -> TL parameters, tensor by tensor. GQA kv heads are repeated. QK-norm,
  extra norms, routers are mapped explicitly when present. Untied embeddings keep W_E
  and W_U separate.

## Why a shared helper is cross-cutting
- Factoring RoPE or a norm into a shared helper touches multiple families' paths. A
  regression there breaks several adapters at once, so it needs a peer +1 (T5) and lands
  in a minor, not a patch.

## Extension policy (T4)
- Published weights + published architecture only. No closed-weights adapters, no private
  client forks. Any adapter, including a paid one, lands upstream as a normal public PR.
