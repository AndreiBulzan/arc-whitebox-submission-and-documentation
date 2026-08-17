# Preregistration: low-rank depth-closure MLMC R2

Date: 2026-08-09

R1 proved the finite-population telescope identity but killed a purely
diagonal Gaussian tail: its optimum under 27.2B was ordinary 25-basis
sampling with `1.19e-6` extra MSE.

R2 is the only immediate covariance-enriched successor.  At every exact
prefix state, fit the oracle-best rank `r in {4,8,16}` covariance factor and
an exact diagonal residual.  Propagate that diagonal-plus-low-rank covariance
through the supplied tail weights and ReLU marginal moments.  ReLU gates
transport the low-rank factor; the diagonal residual is repaired to preserve
each marginal variance.

The closure remains only a control.  Its deterministic bias cancels exactly
in the depth telescope, whose finest level is the literal complete-basis
output.

## Evidence boundary

Target-free component capture on the same four Full and four Generated
development weights as R1.  No targets, FlopScope session, physical row,
Mini100 row, package, upload, submission, or remote action.

The exact eigenspace fit is a capacity oracle.  A production successor would
need a fixed randomized range approximation of the factor, priced in the
post-capture optimizer.  No production claim may inherit the oracle factor
without that extra gate.

## Gates

1. Telescoping identity error below `2e-6` for every rank.
2. Under conservative prefix, randomized-range, and closure prices, a rank
   must project below `1e-6` extra raw MSE at no more than 27.2B on both
   families.  The stretch gate is `6e-7`, leaving room for the K129 residual
   on a path to adjusted `8e-8`.
3. If even exact top-r factors fail the `1e-6` gate, close every cheaper
   rank-r implementation at that rank.  Literal full covariance is not a
   score-floor successor because one covariance sandwich costs roughly one
   complete-basis layer.

