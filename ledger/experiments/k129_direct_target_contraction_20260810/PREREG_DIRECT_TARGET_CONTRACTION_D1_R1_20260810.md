# Preregistration: direct target-contraction replay D1/R1

**Date:** 10 August 2026  
**Evidence boundary:** target-free replay followed by post-seal scoring on the
already-open 16 Full plus 16 Generated development rows  
**Authorized actions:** offline replay only; no physical row, official Mini100
row, package, upload, submission, or remote action

## Question

Can a small set of exact Haar final-output audits estimate the one missing
normal-equation contraction of the 129 already-evaluated R90 basis endpoints
well enough to improve adjusted score after paying for the audits?

For one network let `Y` be the `129 x 256` repaired endpoint matrix, `y0` its
uniform row mean, and `S = Y - y0`.  The observable Gram matrix is

`A = S S^T / 256`.

If `theta` is the unknown expectation, the target-optimal contrast weights
solve

`A delta = b`, where `b = S (theta - y0) / 256`.

An exact final-output audit `D` has `E[D] = theta`, hence

`b_hat = S (D - y0) / 256`

is an unbiased observation of the missing contraction.  This replay tests
that observable directly.  It does not fit individual off-design responses.

## Frozen inputs

The target-free stage must verify the existing receipt hashes before reading:

- `runtime/artifacts/k129_r90_q0_r10e_targetfree_20260809.npz`;
- `runtime/artifacts/k129_haar_direct_mean_final_risk_r10d_targetfree_20260809.npz`.

The first contains the repaired R90/q0 prediction and all 129 repaired endpoint
vectors.  The second contains two independent cumulative Haar audit banks at
counts `1, 2, 4, 8, 16`.  The audits are multiplied by the live output scale
stored in the R90 capture.  Row IDs must agree exactly.

## Frozen candidate family

For every row, audit count, and replica:

1. center the endpoint matrix to obtain `S`;
2. eigendecompose `A` on its nonconstant support;
3. solve `(A + relative * scale * I) delta = b_hat`, where
   `scale = trace(A) / 128` and
   `relative` is one of
   `1e-6, 3e-6, 1e-5, 3e-5, 1e-4, 3e-4, 1e-3, 3e-3,
   1e-2, 3e-2, 1e-1, 3e-1, 1, 3, 10, 30, 100`;
4. test the raw solution and positive ray-bounded solutions with maximum
   basis weights `2/129`, `4/129`, and `8/129`;
5. form the output correction `c = S^T delta`.

Numerically null eigenmodes are dropped.  No benchmark target may influence
the ridge, weight mode, audit count, replica mode, or scalar shrink.

## Cross-replica selection

For corrections `c0,c1` and audit residuals `e0=D0-y0,e1=D1-y0`, estimate
the gain of a single-replica spelling by cross-fitting:

`G_single(beta) = mean_r [2 beta <cr,e(1-r)>/256
                          - beta^2 ||cr||^2/256]`.

Fit one global positive scalar `beta` from the cross moments and clip it to
`[0,2]` for raw weights and `[0,1]` for bounded weights.

For the two-replica average `cbar=(c0+c1)/2`, use

`G_average(beta) = beta [<c0,e1>+<c1,e0>]/256
                    - beta^2 ||cbar||^2/256`.

All moments are aggregated across the 32 rows.  A cell is eligible only when
its estimated gain is positive separately on Full and Generated.  Among
eligible cells, select the one with the smallest projected adjusted-score
ratio using:

- frozen baseline raw MSE `2.204411347058278e-7`;
- frozen R90 effective compute `137.8297337635B`;
- `1.08B` per complete Haar audit basis;
- one audit bank for the single-replica production spelling and two for the
  averaged spelling.

For a selected single-replica cell, `replica0` is the frozen deploy spelling;
`replica1` is a disjoint diagnostic.  The selector may instead freeze the
two-replica average if its projected score is lower.

## Post-seal scoring and gates

Only after all predictions, coefficients, and the selector are hashed may the
already-open development targets be read.  Report raw MSE ratios, separate
family ratios, rows improved, and projected adjusted score for:

- the frozen deploy spelling;
- its disjoint replica when applicable;
- the two-replica average;
- the target-aware best cell as a capacity diagnostic only.

Advance to an official-Mini100 capture only if the frozen target-free deploy
spelling:

1. improves both Full and Generated;
2. projects below the R90 adjusted reference;
3. uses no more than four audit bases per deployed replica; and
4. either reduces pooled raw MSE by at least 7% or projects at least 1% below
   the R90 adjusted reference.

If only 8- or 16-audit cells carry signal, record the statistical capacity but
kill the production route on score economics.  If cross-replica estimated gain
is nonpositive, kill the direct-contraction bridge without opening targets for
selection.

## Prior-art boundary

- `final_hidden_rao_blackwell_20260729` Gaussianized a PCA summary of the final
  hidden cloud.  It did not observe or estimate `S theta`.
- `projected_total_contraction_20260808` tested the same projection principle
  for packet/source audit vectors in a selected 16-source span; its variance
  was too high.  This is adverse evidence, but the target, audit unit, and
  129-endpoint contrast span differ.
- `k129_quenched_arccos_teacher_20260809` already captured the exact Haar
  outputs used here.  R10D/R10E used them for layer-31 calibration and a scalar
  endpoint blend.  They did not contract the audits against the full centered
  endpoint matrix or solve its normal equations.

The materially new element is therefore the observable and its use, not the
audit trajectories.
