# Triage note: issue #1096 (ActivationCache device-placement)

- Reporter: Chi Wong-Delacroix (chi.wongd@gmail.com), independent researcher, writes
  clean minimal reproducers.
- Filed: 2026-04-29 with a minimal reproducer. First response 2026-04-30 (within SLA),
  repro confirmed, PR invited.

## Symptom
- `run_with_cache` on a cuda model, then indexing the cache on cpu, raised a device
  mismatch on some stacking helpers (cache tensors on cpu while run on cuda).

## Resolution
- Chi opened PR #1099 (2026-05-04): a small, well-scoped fix + test. Reviewed as a real
  human review (no LLM code merged as-is, T5), merged with green CI 2026-05-05, credited
  to chi-wd. Shipped v2.15.0.
- This case motivated the explicit `device=` argument in the ActivationCache signature
  change (see design_ActivationCache_signature_change.md).
