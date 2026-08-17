# K12 analytic connected-moment shrinkage R1 preregistration

Date: 2026-08-09

Evidence class: component development diagnostic on the already-existing
256-network public pair-teacher bank.  This bank has been used by earlier
work, so this experiment is not pristine or broad challenge-family evidence.

## Question

Can the noisy final-preactivation moments from the proper isolated K12 cloud
be shrunk toward the target-free full-covariance analytic closure strongly
enough to recover the previously observed connected-teacher capacity?

The prior self-Gaussian experiment used the K12 moments without analytic
shrinkage.  The prior student fitted a generic map from final K12 summaries.
This test instead uses the natural empirical-Bayes moment estimator and then
applies the exact Gaussian ReLU mean.

## Target-free capture

For each of the already frozen 256 weight tensors, run the incumbent analytic
full-covariance recursion with the production screen schedule and save:

- the calibrated analytic final-preactivation mean;
- the analytic final-preactivation marginal variance;
- the raw analytic mean before its incumbent affine calibration.

The capture must not open `target_h31`, the pair teacher, or any benchmark
truth.  It performs no FlopScope run, physical row, package, upload, or
submission.

## Frozen estimator family

Use the existing isolated K12 per-basis first and second radial moments,
including the incumbent `lambda=0.0075` endpoint shift.  Let `m_s,q_s` be
their sampled first and second moments and let `m_a,q_a` be the analytic
moments.

Test two target-free shrinkage families:

1. fixed sample-retention factors
   `r_mu,r_q in {0,0.1,0.25,0.5,0.75,1}`;
2. positive-part multivariate James--Stein retention, separately for first
   and second moments, with
   `kappa_mu,kappa_q in {0.25,0.5,1,2,4}` and sampling-noise traces estimated
   from the 12 basis replicates.

For both families use analytic variance multipliers
`{0.75,0.9,1,1.1,1.25}`.  Convert the shrunk moments through the exact
rectified-normal mean.  Blend that readout with direct K12 using one scalar
coefficient fitted on the first 160 networks and clipped to `[0,1.5]`.

Select one candidate by lowest raw MSE on the next 48 development networks.
Open the final 48-network held split once.  No held-dependent retuning is
allowed.

## Gate

Continue to fresh challenge-family capture only if the selected held result
satisfies all of:

- raw MSE at most `1.0e-6`;
- row-MSE p95 at most `2.0e-6`;
- ratio below `0.90` versus direct K12;
- all predictions finite.

A failure closes this analytic moment-shrink spelling, not all all-layer
connected-state estimators.

