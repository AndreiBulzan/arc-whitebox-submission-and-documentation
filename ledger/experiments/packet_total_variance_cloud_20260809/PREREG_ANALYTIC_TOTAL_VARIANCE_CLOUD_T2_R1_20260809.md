# Analytic spherical-total variance cloud T2 R1 preregistration

Date: 2026-08-09

Evidence class: target-free component oracle. No benchmark target,
FlopScope/physical prediction, upload, or submission.

## Why T2 is distinct from the killed T1 and R6 recurrences

V1 showed that the exact full-covariance teacher's centre-averaged marginal
variance vector preserves 97.9% of the final packet correction. T1 failed
because it substituted the Kerdock cloud's own directionally biased second
moment for the spherical total. R6 failed because it recursively evolved one
shared covariance using an inaccurate shared-covariance assumption.

T2 does neither. It computes a separate target-free full-covariance *spherical
moment closure* once per network, starting from `N(0,I/256)`, and retains only
its 256 preactivation second moments at each layer. Those are already the kind
of analytic `seconds` paid for in R90. The K129 cloud carries conditional means;
the law of total variance supplies one shared marginal variance vector:

`v_l = max(r2 * sphere_pre_second_l - mean_c(m_pre_l^2), eps)`.

Then all K129 conditional means receive the exact univariate Gaussian ReLU
moment. No covariance is attached to a centre and the analytic covariance is
not conditioned on, or evolved from, the cloud.

## Frozen data and numerics

- Same sealed Full8/Generated8, 64-replicate packet R2 population.
- Same 66,048 oriented K129 nodes, packet constants, 32 actual weight matrices,
  and selected layers as T1.
- Analytic closure: float64, quadrature order 8, input mean zero and covariance
  `I/256`; record the preactivation second vector before every ReLU.
- Cloud: float32 matrix products with TF32 disabled; ReLU moments finite.

Primary method: `analytic_total_variance`. Diagnostic method:
`analytic_fixed_radial_variance = (r2-rho^2)*sphere_pre_second`.
No fitted scale, blend, shrinkage, layer selection, or target use is allowed.

## Gates

Use the same replicate-noise-corrected packet correction metric as T1.
Primary pass requires:

- final pooled fidelity >= 0.70;
- final Full and Generated fidelity >= 0.60 each;
- final pooled cosine >= 0.85;
- layer-8 pooled fidelity >= 0.60;
- all values finite and q0 provenance/hash checks pass;
- fraction of primary variance coordinates clipped at the floor <= 0.05.

Near pass: final pooled fidelity >= 0.50 and cosine >= 0.75. A pass licenses
post-seal target scoring and an R90 graph-integration/cost design. It remains a
component result, not a Mini100 or score receipt.
