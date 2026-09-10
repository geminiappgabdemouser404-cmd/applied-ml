# v2.16.0 erratum: `ActivationCache` device API

The v2.16.0 release materials incorrectly reported that an explicit `device=` argument had
shipped for `ActivationCache`, `stack_head_results`, and `stack_activation`.

The published `transformer-lens==2.16.0` wheel retains the legacy public signatures:

- `ActivationCache(cache_dict, model, has_batch_dim=True)`
- `stack_head_results(layer=-1, return_labels=False, incl_remainder=False, pos_slice=None, apply_ln=False)`
- `stack_activation(activation_name, layer=-1, sublayer_type=None)`

The existing supported device controls are `run_with_cache(device=...)`, which selects where
activations are cached, and `ActivationCache.to(device)`, which moves an existing cache. The
proposed constructor and stacking-helper arguments remain unshipped and must not be relied on.
