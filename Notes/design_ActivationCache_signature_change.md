# Design note: ActivationCache signature change (cross-cutting, v2.16.0)

Flagged at the 2026-05-19 maintainer sync as cross-cutting; targeted for a minor (not a
patch); merged after Rehan's +1 (2026-05-21), shipped in v2.16.0.

## Motivation
- Device-placement footguns (a cache built on cpu while the model runs on cuda, #1099)
  and inconsistent stacking helpers pushed toward an explicit `device=` on the cache
  constructor and stacking methods, plus a clearer return contract.

## Proposed change
- Add an explicit optional `device` argument to `ActivationCache` and to
  `stack_head_results` / `stack_activation`. Default preserves current behavior
  (infer from the first cached tensor) so existing code keeps working.
- Deprecate the silent device inference in a later minor with a warning first.

## Why cross-cutting (needs +1)
- ActivationCache is depended on by SAE workflows, patching, and downstream tools; a
  signature change is a public-surface change. Per policy it needs a peer +1 and lands
  in a minor with a deprecation path, never a patch.

## Compatibility
- Backward compatible in v2.16.0 (new arg optional). Deprecation warning for the old
  inference path scheduled for a later minor. Documented in the release notes and docs.
