# Spherical Sobol/RQMC R2 replication preregistration

Evidence label: **component**.

R1 produced a promising pooled result but failed its deliberately strict
two-row-half gate.  R2 is a disjoint, larger replication.  The row lists and
gate below are fixed before opening any R2 targets.

## Fixed construction

- Width 256, depth 32, dense float32 propagation.
- Exactly 4,096 positive angular nodes and their 4,096 negatives for each
  method (8,192 propagated nodes).
- Control: eight Kerdock bases from each of the two production orientations.
- Candidate: the first `2^12` points of a 256-dimensional Owen-scrambled
  Sobol net (`seed=2026072901`), mapped componentwise through the inverse
  Gaussian CDF and normalized to the unit sphere; exact antipodes are then
  appended.
- Both methods receive the same analytic layer-1 marginal mean/variance
  repair and the same Gaussian radial mean restoration.  No target-dependent
  choice or coefficient is allowed.

## Fixed disjoint rows

- Full: `2..17` and `102..117`.
- Generated: `2..17` and `66..81`.

These 32+32 rows do not overlap R1.

## Fixed decision

For each family, report the pooled noise-corrected MSE ratio, four consecutive
eight-row block ratios, row win fraction, median row ratio, p90 row ratio, and
worst row ratio.

Continue only if:

1. both pooled candidate/control ratios are at most `0.80`;
2. every fixed eight-row block ratio is at most `1.00`; and
3. all predictions are finite.

This remains a simplified angular-quadrature component screen, not a
production-estimator receipt.  Passing earns a production-repair integration
study; failing kills this Sobol construction.

