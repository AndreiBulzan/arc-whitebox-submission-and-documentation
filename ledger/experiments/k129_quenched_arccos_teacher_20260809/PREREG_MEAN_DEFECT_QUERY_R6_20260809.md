# Preregistration: layer-31 mean-defect query R6

Date: 2026-08-09

## Fixed hypothesis

R5 localizes the successful Gaussian-moment teacher completely to the
256-dimensional layer-31 mean.  Exact dense mean plus ordinary K129 covariance
reduces corrected MSE by 59.81%; ordinary K129 mean plus exact dense covariance
is neutral.  Recursive Gaussian means point in the wrong direction.

R6 therefore estimates only the tiny mean defect, retaining the already-good
complete-K129 covariance.  This is materially lower variance than estimating
512 rectified feature means or a full covariance from the same query rows.

## Query designs and fixed shrinkage

Use the exact R3 independent-Haar and globally rotated-K129 seeds, with two
independent replicates and cumulative complete-basis counts.  For each query
mean `m_r` and canonical mean `m_0`, form replicate defects `d_r=m_r-m_0`.

The primary target-free estimator uses positive-part replicate shrinkage:

`T=max(dot(d1,d2),0)`, `V=norm(d1-d2)^2/4`,
`alpha=T/(T+V)`, `m_hat=m_0+alpha*(d1+d2)/2`.

Controls seal the raw two-replicate average and a fixed shrink grid
`alpha in {0.01,0.03,0.1,0.2,0.3,0.5,0.75,1}`.  Every resulting synthetic
feature, ridge weight, and actual-output prediction is sealed before targets.

## Gates

A production-affordable setting means at most 16 complete bases per replicate
(16,384 total rows for two replicates) for the first pass.  Continue if the
fixed empirical-Bayes rule reaches at least 20% pooled raw reduction with no
family reversal, or if a fixed shrink cell shows at least 35% component
capacity at that count.  Counts above 32 bases per replicate are scaling
diagnostics only unless their score arithmetic is independently favorable.

No physical row, Mini100 row, FlopScope session, package, upload, submission,
or remote action is authorized.
