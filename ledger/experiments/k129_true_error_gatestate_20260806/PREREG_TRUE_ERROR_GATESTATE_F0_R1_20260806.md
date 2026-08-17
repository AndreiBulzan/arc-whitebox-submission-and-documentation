# K129 true-error gate-state F0 R1

Date: 2026-08-06

Evidence sought: a **component development diagnostic** using the already
sealed, target-free 40-network q0 gate-state capture.  This experiment runs
no estimator, FlopScope session, package, upload, or remote action.

## Prior-work boundary

The same capture failed to predict the four-complete-frame teacher delta.
That does not determine whether it predicts the actual q0 quadrature error:
the teacher is another finite quadrature rule, not ground truth, and its
unpredictable component can be irrelevant to q0 risk.  Prior learned heads
used endpoint/aggregate summaries; this capture contains per-neuron gate
occupancy, normalized margins, antithetic divergence, per-basis dispersion,
skew, and pooled multidepth context from the literal q0 trajectory.

This is only a cheap signal test.  A pass licenses a new capture on the
independent 500-network high-precision corpus; it does not license Mini100,
an estimator edit, or a deployment claim.

## Frozen rule

- Inputs: `f0_gatestate_sketch_r1_targetfree_20260806.npz`, unchanged.
- Label: exact available final target minus the sealed q0 prediction.
- Split: five grouped folds by network position.  Both families are pooled
  for fitting; no coordinate from a held network enters its fit or feature
  normalization.
- Models, fixed before target access:
  1. ridge, alpha 30;
  2. histogram gradient boosting, 300 trees, depth 6, learning rate .07,
     L2 1.
- The model is shared across all output coordinates.  No MLP identifier,
  output-coordinate identifier, target-derived router, coefficient scan, or
  post-hoc shrink is allowed.
- Candidate = q0 + out-of-network-fold predicted residual.

Generated metrics report both observed MSE and the standard scalar
label-noise correction.  Selection uses one model unchanged across both
families.

## Gate

Continue only if one fixed model simultaneously achieves:

- corrected/raw candidate-to-q0 MSE ratio at most 0.85 in each family;
- at least 12 of 20 networks improved in each family;
- observed row-ratio p95 at most 1.50 in each family; and
- finite predictions with maximum absolute correction at most 0.01.

Failure kills this exact q0 gate-state-to-true-error spelling.  Do not tune
models, alphas, feature subsets, clipping, or folds on these opened rows.

