# Preregistration: frozen d16/right/24 broad falsifier

Evidence sought: **broad statistical** raw-accuracy evidence on the reused
Full100 and Generated128 banks. Deployment compute remains a **projection**.
No FlopScope row, package, upload, or submission is part of this experiment.

## Capsule prior-art preflight

Queries covered `fourframe`, `multiframe`, `right frame`, `alternate frame`,
`coreset`, `herding`, `OMP`, `sparse scout`, `depth 16`, `right24`, and the
equivalent operation “q0 high-fidelity anchor plus a low-fidelity frame
difference.” The nearest controlling results are:

- `fourframe_static_coreset_r1_postseal_20260802.json`: a static K129 mixture
  across complete frames was killed (`2.80x`/`3.31x` the ideal-four error).
- `direct_sparse_d6_scout_r6_postseal_20260802.json`: the shallow direct
  sparse rule reached raw ratios `0.805`/`0.858`, insufficient after cost.
- `frozen_frame_medoid_r18_postseal_20260802.json`: per-network frame
  selection reversed on selection-disjoint rows (`1.037`/`1.031` raw).
- `oof_highframe_medoid_r19_postseal_20260802.json`: out-of-fold medoid
  selection was also killed.
- `multidepth_sparse_scout_r9_postseal_20260802.json`: the materially
  different global d16/right/24 rule was selected without challenge targets
  and obtained pilot raw ratios `0.734`/`0.754` with projected support-layer
  cost ratio `1.096024`.
- `d16_right24_scout_r10_targetfree_20260802.npz`: the exact support, affine
  weights, and scalar coefficient were frozen before this broad test.

Outcome: **materially new observable**. This does not repeat the killed
static-coreset or adaptive-selector spellings. It applies one already-frozen,
global correction to 228 rows that did not select it.

## Fixed estimator spelling

For each MLP, use the paid complete q0 endpoint as anchor. At depth 16, form
the frozen affine combination of 24 right-frame per-basis diagonal
continuations, subtract the complete q0 depth-16 proxy mean, multiply by the
already-frozen scalar, and add that difference to the q0 endpoint. No support,
weight, coefficient, depth, or frame may change after targets are opened.

## Target ceiling and decision

At R27's approximately `1.1936e-7` projection and the conservative support
cost ratio `1.096024`, unchanged calibration requires a worst-family raw ratio
`<=0.84084` for `1.1e-7`, and `<=0.76440` for `1.0e-7`.

Promote to physical implementation research only if all hold:

1. both pooled raw ratios are `<=0.80`;
2. the 95th percentile paired-bootstrap raw ratio is `<=0.84084` in both
   families; and
3. at least 60% of rows improve in each family.

Otherwise kill this exact d16/right/24 rule. A pass is still not remote proof;
it earns exact deployment pricing and an independent-bank confirmation.
