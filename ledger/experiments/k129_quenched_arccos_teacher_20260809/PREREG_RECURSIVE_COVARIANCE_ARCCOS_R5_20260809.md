# Preregistration: recursive covariance arc-cosine bridge R5

Date: 2026-08-09

## Fixed question

R4 showed that the exact layer-31 mean and full covariance, passed once
through the rectified-normal formula for 512 synthetic final rows, retain
59.62% raw reduction.  The diagonal covariance retains none.  Complete K129
moments are globally extremely close to the dense moments (about 0.05--0.08%
relative mean error and 0.5--0.8% covariance error) but their small signed
error points in the wrong correction direction.

R5 asks whether a deterministic full-covariance Gaussian recursion through
the actual first 31 supplied layers predicts that small target-moment defect
well enough to calibrate the already-observed K129 moments.

## Prior art and new observable

The anchored-tail full-Gaussian closure failed as a direct output estimator,
even over two layers.  That negative remains controlling for direct closure.
R5 is reopened only because R4 established a different sufficient statistic:
the closure supplies a *target covariance total* for a stable shared basis
calibration whose exact-moment ceiling is strong.

## Variants

Starting at exact input `N(0,I)`, recursively propagate mean and covariance
through layers 1--31 using:

- the existing exact bivariate rectified-Gaussian moment formula;
- the existing first-Hermite off-diagonal plus exact marginal diagonal rule.

Compute synthetic target features from those moments.  Also seal convex
moment blends of the bivariate recursion and observed complete-K129 moments
at K129 fractions 0.25, 0.50, and 0.75.

Diagnostic ceilings isolate the two moment errors:

- exact dense mean plus K129 covariance;
- K129 mean plus exact dense covariance.

Every ridge prediction is sealed before benchmark targets are reopened.

## Gate

Continue only if a fully target-free recursive or recursive/K129 blend reaches
at least 20% pooled raw reduction with neither family reversing.  If only the
dense-covariance diagnostic passes, recursive covariance accuracy is
insufficient and the next bridge must estimate the covariance defect rather
than retune Gaussian closure.  Projected production arithmetic must remain
below 10B before a physical implementation is licensed.

No physical row, Mini100 row, FlopScope session, package, upload, submission,
or remote action is authorized.
