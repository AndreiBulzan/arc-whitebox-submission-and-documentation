# Packet moment-pilot P1 R1 preregistration

Date: 2026-08-09

Evidence class: target-free component oracle. No benchmark expectation,
FlopScope prediction, upload, package, or submission.

## Mechanism

Direct sparse packet-output averaging failed because its signed vector variance
is too large. P1 uses packet evaluations only to estimate the positive global
preactivation second moments needed by the successful V1 shared-marginal
representation.

For `k` complete Kerdock bases, draw one Gaussian packet perturbation per line
and propagate the antipodal pair

`x = rho*u + tau*z`, `-x`.

At every layer record only

`T_lj = mean_packet_nodes(z_lj^2)`.

Separately propagate conditional packet means for all 66,048 canonical
oriented centres. At layer `l`, set

`v_lj = max(T_lj - mean_centres(m_lj^2), eps)`

and apply the exact univariate Gaussian ReLU mean. The packet pilot is thereby
Rao--Blackwellized through the entire canonical population; its output vectors
are never directly averaged into the estimate.

## Frozen experiment

- Sealed Full8/Generated8 64-replicate Gaussian-packet R2 population.
- Complete K129 geometry and actual 32 supplied matrices.
- Nested uniformly permuted basis supports `k = 4, 8, 16, 24, 32, 64, 129`.
- Eight independent support/noise replicates, seeds fixed in source.
- One antipodal packet pair per selected line (`2*k*256` pilot rows).
- Float32 matrix products, TF32 disabled; finite exact Gaussian ReLU means.
- Selected output layers 1,2,4,8,16,24,32.

No support, seed, amplitude, layer, or shrinkage selection may use packet truth
or benchmark targets. All configurations are reported.

## Metrics

For each `(k, replicate)`, compare the deterministic conditional-mean output
with the independent sealed packet replicate mean. Subtract the estimated
packet-mean Monte Carlo variance from residual MSE and correction energy.
Report pooled/Full/Generated fidelity and cosine, plus medians, tenth
percentiles, and pass counts across the eight pilot replicates.

## Gates

Primary production-capacity pass: some `k <= 16` has all of:

- median final pooled fidelity >= 0.70;
- median final pooled cosine >= 0.85;
- median Full and Generated fidelity >= 0.60 each;
- at least 6/8 replicate fidelities >= 0.50;
- median layer-8 pooled fidelity >= 0.60;
- median clipped variance-coordinate fraction <= 0.05.

Fallback pass: the same final pooled fidelity/cosine and family requirements at
`k <= 32`. Configurations `k=64,129` are diagnostic capacity ceilings, not
production passes.

A primary/fallback pass licenses post-seal target scoring and a meter-aware R90
integration design. Failure even at `k=129` kills one-pair packet moments as a
source for this recurrence, but not a lower-variance moment cubature or exact
deterministic spherical second-moment rule.
