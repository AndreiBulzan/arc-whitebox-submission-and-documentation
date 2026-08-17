# Preregistration: learned orientation selector R1

Date: 2026-07-29

Evidence sought: **component** on the already-open Full200 and Generated128
eight-orientation signed-endpoint atlases.  Economics remain a
**projection**.  No physical row, FlopScope session, package, upload, or
remote action is in scope.

## Question

Can one inference-lawful, supervised estimate of the unknown signed endpoint
turn the existing three-basis eight-orientation pilot into a materially
better selector of the two paid orientations?

## Frozen split

- Test Full MLPs are exactly the 100 indices in
  `k146_m17_gpu_broad_full100_generated64_targetfree_v2_20260727.npz`.
- Training Full MLPs are the other 100 indices in the Full200 atlas.
- Generated transfer is exactly the 64 generated indices in that same K146
  capture.
- No Generated row is used for fitting, normalization, regularization, or
  model selection.

The split is by complete MLP.  Output coordinates from one MLP never cross
the train/test boundary.

## One fixed model

For each output coordinate, take the `8 x 3` endpoint values from the first
three bases of the frozen S109 ordering.

1. Let `location` be their 24-value mean.
2. Sort the eight orientation triples by their three-basis mean.  This
   canonicalization makes the feature invariant to an arbitrary permutation
   of orientation labels.
3. Flatten the 24 sorted values after subtracting `location`.
4. Apply one shared standardized ridge regression to predict
   `exact_signed_target - location`.

The same 24 coefficients are used for every output coordinate, so permuting
output coordinates merely permutes predictions.  Ridge strength is fixed at
`0.01` in the standardized mean-square objective.  There is no sweep,
cross-validation, nonlinear expansion, family-specific coefficient, or
post-hoc blend.

At inference, predict the 256-vector signed target, then choose the
orientation pair whose equal three-pilot-basis mean has minimum vector MSE
to that prediction.  The selected pair is evaluated using the frozen S109
endpoint.  The same pair's S33 endpoint is reported only as a K84 deployment
diagnostic.

## Gate

Promote only if the learned S109 selector:

- materially improves the existing trimmed-consensus selector;
- has aggregate signed-MSE ratio at most `0.50` against K146 on both untouched
  Full100 and Generated64; and
- has row-ratio p95 at most `1.50` on both families.

Otherwise kill this spelling without physical or packaging work.

