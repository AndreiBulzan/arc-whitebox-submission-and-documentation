# Preregistration: K129 depth-closure MLMC R1

Date: 2026-08-09

## Question

Can the accurate complete K129 finite-population total be reconstructed below
the 27.2B score floor by telescoping exact prefixes against a deliberately
cheap diagonal-Gaussian tail closure?

This is not a closure used as the estimator and not the earlier output-level
width correction.  For complete basis `b`, define `G_l(b)` as:

- propagate the 512 antipodal basis rows exactly through layer `l`;
- reduce their Gaussian-radial marginal mean and variance;
- propagate those moments through layers `l+1..32` with a diagonal Gaussian
  closure.

Set `G_0` to the deterministic diagonal closure from `N(0,I)`, and set
`G_32(b)` to the exact radially-scaled basis output.  Then, coefficient by
coefficient,

```text
G_0 + sum_{l=1..32} [G_l(b) - G_{l-1}(b)] = G_32(b).
```

The closure's bias therefore cancels identically.  Sampling different basis
subsets for different increments gives an exactly unbiased estimator of the
finite K129 total.  Only the finite-population increment variances determine
the extra MSE.

## Evidence boundary

R1 is a target-free GPU component capture on four Full and four Generated
development rows.  It opens weights only and stores all 129 basis values at
all 33 levels.  It uses `runtime/.benchmark_lane.lock` but opens no
FlopScope session, physical row, Mini100 row, package, upload, submission, or
remote action.

A separate post-seal scorer may open the already-development targets.  All
compute and score values remain projections until a capsule-native estimator
and physical receipts exist.

## Frozen geometry and numerics

- canonical orientation-zero K129 bases from the capsule compact artifact;
- all 512 antipodal rows per basis;
- literal dense float32 supplied-weight propagation, no learned parameters;
- float32 diagonal closure using exact univariate rectified-normal moments;
- float64 terminal serialization and reductions;
- levels `0,1,...,32`.

## Gates

1. Maximum telescoping identity error below `2e-6` per coordinate.
2. Plain exact K129 must leave a plausible floor margin: pooled raw MSE below
   `5e-7` on each development family.
3. A target-free allocation frozen on either family and transferred to the
   other must project below `1e-6` raw at no more than 27.2B.  The stretch
   gate is below `8e-7` raw, corresponding to adjusted `8e-8` at the floor.
4. Reject the diagonal depth-telescope class if even the target-free oracle
   allocation cannot project below `1e-6` raw on both families.  Such a kill
   does not close a full-covariance or mixture-valued telescope.

