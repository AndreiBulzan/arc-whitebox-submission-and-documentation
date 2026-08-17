# Preregistration: frozen near-complete polar grid on official Mini100

Date: 2026-08-05

## Question and target ceiling

Can one of the already-frozen literal sparse-polar arms at size 80, 96, or
104 retain enough of the complete q0/polar cancellation on official Mini100
to produce a projected adjusted score at or below `1.10e-7`?

The rules, supports, coefficients, and output shrink `alpha=0.5` were fixed
on the old Full16/Generated16 pilots before their Mini100 predictions were
computed.  The existing R5 Mini100 run evaluated only size 64.  This attempt
therefore fills a missing regression cell; it does not refit any rule on
Mini100.

Using remote R31 (`1.1577475730232515e-7` at `141.1403492625B`) and the
existing conservative cost model

```text
C(m) = 141.1403492625B + 5B + 0.999B * m,
```

the modeled costs are `226.060B`, `242.044B`, and `250.036B`.  Size 96 is
the primary economic target: it needs a Mini100 raw ratio no larger than
about `0.554` to project below `1.10e-7`.  Size 80 needs about `0.594`.
Size 104 is a transfer diagnostic because its modeled cost is already above
the conservative `246.95B` deployment ceiling unless its raw ratio is
better than the complete-arm rule.

## Prior-art preflight

Queries covered `polar sparse`, `near complete`, `leave out`, `basis drop`,
`partial frame`, `weighted cubature`, `coreset`, `MLMC`, and sizes
`80/96/104/112/116/120` at the production polar call site.

Nearest controlling artifacts:

- `runtime/artifacts/k129_polar_sparse_grid_r5_targetfree_20260804.npz`:
  literal Full16/Generated16 predictions for sizes 64/80/96/104;
- `runtime/artifacts/k129_polar_sparse_grid_r5_postseal_selection_20260804.json`:
  frozen supports, coefficients, alpha, and pilot economics;
- `runtime/artifacts/k129_polar_sparse_r5_mini100_postseal_20260804.json`:
  the size-64 Mini100 failure;
- `runtime/artifacts/k129_paired_polar_endpoint_mlmc_mini100_r2_postseal_20260805.json`:
  the distinct size-24 matched-endpoint MLMC failure.

Outcome: **materially untested frozen cells**.  The negative size-64 and
size-24 results do not determine the near-complete size-80/96 transfer
curve, and no literal Mini100 predictions for these frozen rules exist.

## Fixed evaluation

Capture literal independently repaired polar-arm predictions at sizes
`80,96,104` with the frozen support and coefficient for each size.  Score
only the pre-existing output rule

```text
y = q0 + 0.5 * (polar_sparse - q0).
```

No alpha, support, coefficient, or size-specific correction may be selected
from Mini100 targets.  Report every size.

Promote a size to physical implementation only if all of:

1. its modeled adjusted projection is `<=1.10e-7`;
2. it improves at least `75/100` Mini100 networks;
3. its paired-bootstrap raw-ratio 95% upper endpoint is `<0.80`;
4. it already passed both old pilot families at the same fixed alpha.

This is **broad statistical** Mini100 regression evidence plus a compute
**projection**.  It is not a measured FlopScope whole or remote evidence.

