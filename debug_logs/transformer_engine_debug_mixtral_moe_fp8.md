# Debug Log

---

## 2026-04-14 — FP8 + Expert Parallelism (EP=8, 8×H100)

### Problem 1: Mixed Tensor/DTensor in fused AdamW

```
RuntimeError: aten._fused_adamw_.default: got mixed torch.Tensor and DTensor
```

**Cause:** EP converts expert weights to `DTensor`; other params stay `Tensor`. Fused kernel rejects mixed types.

**Solution:** Split params into separate optimizer groups, disable `fused=True` and `foreach=True`:

```python
dtensor_params = [p for p in model.parameters() if isinstance(p.data, DTensor)]
tensor_params  = [p for p in model.parameters() if not isinstance(p.data, DTensor)]
optimizer = AdamW([{"params": tensor_params}, {"params": dtensor_params}], fused=False, foreach=False)
```

---

### Problem 2: Accelerate DDP rejects DTensor model

```
ValueError: Your model contains `DTensor` parameters, which is incompatible with DDP.
```

**Cause:** `accelerator.prepare(model)` wraps with DDP; DDP rejects DTensor params.

**Solution:** Skip model in `prepare()` for EP path. Do not wrap in DDP.

```python
optimizer, dataloader, scheduler = accelerator.prepare(optimizer, dataloader, scheduler)
```

---

### Problem 3: Shape error with FP8 (Grouped Linear)

```
[rank5]:   File "/usr/local/lib/python3.12/dist-packages/transformer_engine/pytorch/module/grouped_linear.py", line 139, in forward
[rank5]:     inputmats = tex.split_quantize(inp_view, m_splits, input_quantizers)
[rank5]:                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
[rank5]: RuntimeError: /workspace/TransformerEngine/transformer_engine/pytorch/csrc/quantizer.cpp:1114 in function get_scale_shape: Assertion failed: last_dim % MXFP8_BLOCK_SIZE == 0 && (numel / last_dim) % MXFP8_BLOCK_SIZE == 0. MXFP8 requires tensor dims that are divisible by 32 (got shape=(562,4096))

RuntimeError: MXFP8 requires tensor dims divisible by 32 (got shape=(562,4096))
```

**Cause:** We were applying te.fp8_autocast() as an outer blanket wrapper and not setting layer_precision. This meant TE's MXFP8 kernels activated without the model's per-layer context, triggering raw shape assertions on data-dependent token counts.

```
WRONG:
  with te.fp8_autocast(enabled=True):
      outputs = model(**batch)

RIGHT:
  config.layer_precision = ["fp8"] * num_layers
  model = NVMixtralForCausalLM(config, fp8_recipe=DelayedScaling())
  outputs = model(**batch)
```

FP8 activation belongs in the model constructor, not the training loop.

---
