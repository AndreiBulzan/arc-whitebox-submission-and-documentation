# Preregistration: Haar direct-mean final-risk bridge R10D

R10 established a real, stable direct layer-31 mean-calibration capacity, but
its layer-31 state-risk selector over-regularized the basis weights.  R10D
tests the missing target-free observable: every queried Haar trajectory also
supplies its exact mean **after the actual final weight matrix and ReLU**.

This is a development replay on the already-open R9B/R10 bank
(`Full676..691`, `Generated72..87`), not fresh validation.  No benchmark
targets may be read until the capture and target-free predictions are sealed.

## Capture

For the two existing deterministic Haar seed replicas and cumulative complete
basis counts `1,2,4,8,16`, capture both:

- the radial layer-31 state mean; and
- the radial final-output mean after applying the supplied `W32` and ReLU.

The layer-31 count-16 values must associate with R10C to within `2e-6`.

## Fixed parameter family

For each count, fit equality-constrained 129-basis weights from the exact K129
per-basis layer-31 means to

`k129_mean + state_shrink * (haar_mean - k129_mean)`.

Use the fixed grids:

- state shrink: `0.025, 0.05, 0.1, 0.2, 0.3, 0.5, 0.75, 1.0`;
- relative ridge: `1e-6, 3e-6, 1e-5, 3e-5, 1e-4, 3e-4,
  1e-3, 3e-3, 1e-2, 3e-2, 1e-1, 3e-1, 1`;
- raw equality-constrained weights and the existing ray-bounded
  `[0,4/129]` spelling.

For each cell, form one calibrated final correction from each independent
replica.  Select the cell by symmetric cross-replica final risk: replica 0's
calibrated output is compared with replica 1's exact Haar final mean and vice
versa.  A single global nonnegative output coefficient is estimated from the
same cross-products and clipped to `[0,2]`.

Seal three estimator classes per count before targets are read:

1. `calibrated`: the selected direct-mean calibrated correction;
2. `query`: a reliability-shrunk direct Haar final-output correction;
3. `hybrid`: a two-coefficient combination of the calibrated and direct-query
   corrections, using a `1e-6` relative two-by-two ridge.  Reject rather than
   trust a hybrid whose coefficient magnitude exceeds `2`.

For each class seal replica 0, replica 1, and the average of both replicas.
The average coefficients use the observed average-feature second moment and
independent-replica cross-products, so they account for the halved query noise.

## Post-seal scoring and gate

After hashing the target-free capture and predictions, read the already-open
development targets and report corrected MSE ratios, family ratios, row wins,
weight stability, coefficients, and a transparent projection using `1.08B`
incremental effective operations per queried complete Haar basis, relative to
the remote R87 `139.365B` price.

Freeze a candidate for a new disjoint bank if a target-free-selected spelling
has pooled raw ratio at most `0.90`, both family ratios below `1`, stable finite
coefficients, and projected adjusted score below R87.  A weaker result kills
this final-risk selector, not the demonstrated target-aware direct-mean
capacity.

No package, upload, submission, remote action, physical FlopScope row, or
Mini100 row is authorized by this experiment.
