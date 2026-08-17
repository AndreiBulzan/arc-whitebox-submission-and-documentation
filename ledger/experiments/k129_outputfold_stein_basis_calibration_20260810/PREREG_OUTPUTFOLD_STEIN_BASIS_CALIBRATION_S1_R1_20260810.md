# Output-fold Stein basis calibration S1/R1 preregistration

**Date:** 2026-08-10  
**Evidence:** target-free candidate seal followed by official-Mini100 public
selection and untouched-holdout transfer  
**Parent:** exact packaged R92

## New statistic

Conditional on the penultimate hidden distribution, the 256 final weight
columns are independent He-Gaussian views.  For a final weight `w` and
`t(w)=E ReLU(w^T H)`, Gaussian integration gives common population moments
across independent output-row folds, including

`E_w[t(w)]`, `E_w[w t(w)]`, and diagonal second-Hermite contractions.

For each of the 129 already-propagated basis endpoint functions, split the
256 supplied final columns into two frozen outer halves.  Within one training
half, split again into four folds and form per-basis estimates of:

- `m0 = mean(t(w))`;
- `m1 = mean((w/tau) t(w))`, all 256 hidden coordinates;
- `m2 = mean(((w/tau)^2-1) t(w))`, all 256 coordinates.

A stable nonnegative basis-weight vector is chosen solely to make the four
training-fold estimates agree.  It is then applied only to the opposite 128
output coordinates.  Swap halves and assemble the final 256-vector.  Thus no
output value is used to choose the basis weights applied to itself.

The candidate correction is composed onto exact R92:

`y = y_R92 + endpoint_basis^T (w_fold - uniform)`.

This is neither the killed analytic-proxy LMCBR nor target-aware held-output
E1.  It uses no target, closure total, off-design row, learned network, or
new trajectory.

## Frozen design

- outer/inner output permutation seed: `2026081061`;
- two outer halves of 128 outputs;
- four equal inner folds of 32 outputs;
- feature families: `m0`, `m1`, `m2`, `m0_m1`, `m0_m2`, `m1_m2`,
  `m0_m1_m2`;
- ridge multipliers: `0.001,0.003,0.01,0.03,0.1,0.3,1,3,10,30,100`;
- ESS minima: `64,96`;
- maximum basis weights: `2/129,4/129`;
- one shared basis vector per held output half, never per output coordinate.

Each fold-difference coordinate is divided by its between-basis standard
deviation with a fixed 10%-of-median positive floor.  Ridge is scaled by the
mean nonconstant eigenvalue of the feature Gram.  The unconstrained ray is
shortened only to satisfy nonnegativity, max weight, and ESS.

## Selection and gates

Seal every candidate before targets are opened.  Select one complete cell on
official Mini100 public rows `0:50`; transfer unchanged to holdout `50:100`.

Promote only if:

- public raw reduction at least `2%`;
- holdout raw reduction at least `2%`;
- pooled raw reduction at least `3%`;
- at least `55/100` rows improve;
- all predictions and weights are finite and constraints hold.

Failure kills this exact output-fold moment-consistency spelling.  No
physical run, package, upload, or submission is authorized by the oracle.
