# Packet total-variance cloud T1 R1 preregistration

Date: 2026-08-09

Evidence class: target-free component oracle. No benchmark expectation is
opened and no FlopScope/physical prediction is run.

## Hypothesis

The V1 capacity oracle showed that one shared per-neuron marginal variance
vector per layer retains nearly all of the centre-averaged exact full-covariance
packet correction. T1 tests a lawful way to generate that vector without any
per-centre covariance.

For a homogeneous network and a unit-direction canonical cloud, let `z0_c` be
the canonical preactivation and `m_c` the propagated conditional packet mean.
The packet input has second radial moment

`r2 = rho^2 + d*tau^2`.

Use the canonical K129 cloud to estimate the unconditional preactivation second
moment and the law of total variance to define

`v_j = max(r2 * mean_c(z0_cj^2) - mean_c(m_cj^2), eps)`.

Then update every centre with the exact univariate Gaussian ReLU mean

`m_c <- E[ReLU(N(m_c, v))]`.

This keeps one shared 256-vector `v` and all already-required cloud means. It
does not propagate a covariance, factor, trajectory shadow, or packet sample.

## Frozen target-free evidence

- Sealed Gaussian-packet R2 capture: Full rows 640--647 and Generated rows
  88--95, 64 independent four-point-antithetic packet replicates.
- Complete K129 oriented cloud: 66,048 rows.
- `epsilon=0.20`, the sealed `rho`, `tau`, radial normalization, 32 supplied
  matrices, and selected layers 1,2,4,8,16,24,32.
- Float32 actual network propagation with TF32 disabled, matching the packet
  capture's numerical convention. ReLU moments may be evaluated in float64 or
  float32 but must be finite.

## Frozen variants

1. `dynamic_total_variance` (primary): subtract the current propagated
   conditional-mean square from the canonical radial total second moment.
2. `fixed_radial_variance` (diagnostic): use only
   `(r2-rho^2)*mean(z0^2)` and do not enforce the changing total-moment identity.

No scalar amplitude, shrinkage, layer selection, or target fit is allowed.

## Metrics and gates

Compare each deterministic closure with the sealed packet replicate population.
Use the replicate mean for cosine and subtract the estimated variance of that
mean from correction energy and residual MSE for fidelity.

Primary pass requires at layer 32:

- pooled correction fidelity >= 0.70;
- Full and Generated correction fidelity >= 0.60 each;
- pooled correction cosine >= 0.85;
- layer-8 pooled fidelity >= 0.60;
- canonical q0 association <= 2e-6;
- all values finite.

A near pass is recorded if final pooled fidelity >= 0.50 and cosine >= 0.75;
otherwise the canonical-radial total-variance bridge is killed. Passing licenses
post-seal target scoring and a no-second-cloud analytic-total replacement test;
it does not license a production score or package.

## Execution

Use the shared benchmark/GPU lane lock. Refuse artifact overwrite. Write a
hash-pinned NPZ and JSON under `runtime/artifacts/`.
