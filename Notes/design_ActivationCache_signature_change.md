# Design note: ActivationCache signature change (cross-cutting, v2.16.0)

Flagged at the 2026-05-19 maintainer sync as cross-cutting and targeted for a minor (not a
patch). This proposal did **not** ship in v2.16.0. The published v2.16.0 wheel retains the
legacy `ActivationCache`, `stack_head_results`, and `stack_activation` signatures.

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

## Status and compatibility
- Unshipped. Do not describe this API as available until the implementation and its regression
  coverage land together.
- When it is implemented, the new argument must remain optional and the old inference path must
  receive a deprecation warning in a later minor before removal.
