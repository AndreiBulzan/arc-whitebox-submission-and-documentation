# K12 late hidden spectral shrinkage R2 preregistration

Date: 2026-08-09

Evidence class: component development diagnostic on the existing public
256-network pair-teacher bank.

## New observable

At the final hidden boundary, retain the complete radial empirical mean and
covariance of the proper isolated K12 particle cloud on the 200 production-
active coordinates.  Also retain the target-free analytic full-covariance
closure mean and covariance on those same coordinates.

Prior K12 work retained only per-basis hidden means and final scalar moments.
Prior checkpoint rejuvenation replaced particles while preserving moments.
This test does neither: it uses the full all-distinct covariance only as a
late Rao--Blackwell/spectral denoising statistic, then applies the actual
supplied final weights.

## Frozen spectral family

For each network whiten the sample covariance by the analytic covariance.
If the generalized eigenvalues are `1+d_i`, soft-threshold their deviations
at `tau_cov in {0,0.025,0.05,0.1,0.2,0.4}` and retain them with
`r_cov in {0,0.25,0.5,0.75,1,1.5}`.

Filter the whitened sample-minus-analytic hidden mean in the same eigenbasis.
Use either every component or only covariance-supported components with
`tau_mean in {0,0.025,0.05,0.1,0.2,0.4}`, and scale the result by
`r_mean in {0,0.25,0.5,0.75,1,1.5}`.

Contract the resulting mean/covariance through the actual final weights,
apply the incumbent calibrated analytic mean as the baseline, and evaluate
the exact Gaussian ReLU mean.  Fit one blend coefficient with direct K12 on
the first 160 networks, choose a single candidate on the next 48, and open
the final 48 once.

## Gate

Continue to a fresh challenge-family capture only if the selected held rule
has raw MSE at most `1.0e-6`, ratio below `0.40` versus direct K12, row-MSE
p95 at most `2.0e-6`, and finite predictions.  A failure closes this late
full-covariance spectral spelling, not earlier-layer connected observables.

