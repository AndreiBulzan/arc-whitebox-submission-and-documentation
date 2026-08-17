# Gauge-equalized packed contraction R1 preregistration

Date: 2026-08-05

Evidence sought: target-free **component** numerical capacity only. No target
opening, score, FlopScope execution, physical row, package, upload, submission,
or remote action is authorized.

## Prior-work boundary

Capsule searches found low-bit cloud screens, gauge-equivariant learned
closures, and polar/gauge frame experiments, but no use of exact positive
diagonal ReLU/product gauge freedom to precondition a packed contraction.
Six-bit three-way packing was arithmetically valid but failed the fixed
precision gate. Seven-bit blocked packing was preregistered but not yet run.

The new spelling is the exact identity

```text
L W = (L D^-1) (D W),       D positive diagonal,
```

with channel scales fixed by the SmoothQuant/equalization rule

```text
D_j = sqrt(max_i |L_ij| / max_k |W_jk|).
```

Zero-range channels use `D_j = 1`. This changes neither the exact real-valued
product nor the estimator target; it only balances the two operands before
affine quantization. It is a capacity screen, not yet a lawful cost claim:
forming the scales and rescaling both operands must be priced before promotion.

## Fixed two-row falsifier

- Full row 0 and Generated row 2;
- existing sealed K64/K16 calibrated-cloud emulator;
- only the previously best `eligible_first24` schedule;
- unsigned 6-bit and unsigned 7-bit affine codes for both operands;
- clipping quantiles remain 0.001/0.999;
- exactly two variants per bit width: unscaled control and the fixed
  `alpha=0.5` equalizer above;
- same-support dense float32 cloud as control;
- no target access and no FlopScope execution.

The seven-bit blocked arithmetic identity is verified independently before
network work. Promote a spelling only if both families have residual variance
ratio at most `0.08` and multilevel discrepancy MSE below `5e-7`. Otherwise
kill it without broadening or tuning alpha.

