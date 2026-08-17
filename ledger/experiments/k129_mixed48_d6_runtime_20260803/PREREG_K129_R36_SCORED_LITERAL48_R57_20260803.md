# K129 R36 scored-literal48 residual screen — R57 preregistration

Date: 2026-08-03

Evidence label: **component**.  This is an offline transfer falsifier, not a
deployment candidate or a remote prediction.

## Prior-art boundary

The old learned-residual screen used K146 endpoint moments, analytic
readouts, and late-weight summaries.  R41/R42 tested final-variance channels;
R45--R47 tested target-free moment corrections and distillation.  No recorded
experiment fits the scored residual directly from the 48 independently
repaired literal D6 atom outputs now available in R36.

## Frozen question

Let `x_j = alternate_j - anchor` for the exact 48 deployed R36 atoms and let
`p_R36` be the exact-literal ridge-0.25 production-associated prediction.
The exact capture must remain within `5e-4` maximum absolute difference of
the earlier float proxy seal.  Fit a single
output-permutation-equivariant ridge correction to `target - p_R36` using
only Full rows.  Compare two predeclared feature sets:

1. the 48 coordinatewise atom differences;
2. those differences plus their 48 per-network output-coordinate means,
   broadcast back over coordinates.

Select feature set and relative ridge from
`{0.01, 0.1, 1, 10, 100, 1000}` solely by four-fold, whole-network Full OOF
raw MSE.  Refit on all Full rows, then open the already available Generated
targets once as a transfer check.  Generated targets do not participate in
fitting or selection.

## Gate

Advance only if Full OOF and Generated noise-corrected raw MSE both improve
R36 by at least 2%, with at least half of the networks improved in each
family.  Otherwise kill this spelling without estimator, physical-row,
package, upload, or submission work.
