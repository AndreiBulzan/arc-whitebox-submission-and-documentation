# TensorSketch connected-K4 R1 preregistration

Date: 2026-07-29

Evidence sought: a static mathematical/component gate first. No target,
GPU, FlopScope, physical row, package, upload, submission, or remote action
is authorized.

## Hypothesis

For a hidden random vector `h in R^256`, a degree-four TensorSketch can
compress

```text
M4 = E[h tensor h tensor h tensor h]
```

and answer a downstream marginal query

```text
<M4, w tensor w tensor w tensor w> = E[(h.w)^4].
```

The hoped-for successor would carry the all-distinct part of connected K4
that the existing `c22/c31` state omits, using at most `5B` current-meter
work.

This is not a marginal-gamma4 denoiser. The blocking question is whether
the sketched joint state has a lawful recurrence through an arbitrary dense
weight followed by coordinatewise ReLU.

## Gates

Stop before a capture if either gate fails:

1. `E[TS4(ReLU(h W))]` must be determined by the retained degree-0..4
   state without reconstructing an `O(width^3)` or larger state and without
   retaining the propagated particle cloud.
2. A conservative current-meter lower bound for the complete recurrence
   must be `<=5B`.

If both pass, the only authorized next step is one tiny target-free capture
and a separately sealed `Full2 + Generated2` component score. It must reduce
matched cheap-cloud/closure raw MSE by at least 20% on each family. Failure
ends the spelling without parameter, hash-width, or rank tuning.

## Frozen local anchors

```text
Full1000 exact marginal K3/K4 capacity score
  3b2403d6009650b4af3460b198eade6f5a7f01ecc827f4662e519da34076e307

Full100 exact final K4 block decomposition
  51a44acdf52904a9cb6f4f9f47c90f74591fd7e4dfd5dbcb343eb1a21e5095e7

actual-K32 target-free endpoint capture
  c80c243453432e8298da7a8867833311b8ff365412ea696427ec43daadd1c383

K32 cloud economics receipt
  7e408522fb943fd3ff5a0529ea9038f5145934288fc68ed580c0375347e34340
```

The Full100 decomposition shows that the omitted 3/4-distinct K4 block has
energy `1.6870807688e-3`, versus `0.3502996103e-3` retained through
pairwise views. The Full1000 oracle shows exact marginal K3+K4 has
`2.366837324e-8` final MSE. These establish capacity, not recurrence.

