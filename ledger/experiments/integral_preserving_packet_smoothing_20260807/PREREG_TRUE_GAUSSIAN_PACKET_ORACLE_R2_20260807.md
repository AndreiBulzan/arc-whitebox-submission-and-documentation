# Preregistration: true Gaussian-packet smoothing oracle R2

Date frozen: 2026-08-07

## Blocking question

Does the exact Gaussian-packet conditional average itself retain at least a
40% raw-MSE reduction on fresh networks at one fixed moderate noise radius?

This is Stage B of the integral-preserving smoothing program.  It measures the
target that a moment closure would need to reproduce.  It does not test any
closure or contest-compatible implementation.

## Frozen construction

For every unit K129 center `u`, set

```text
rho = cos(0.20)
tau = sin(0.20) / sqrt(256)
Y = rho*u + tau*Z,  Z ~ N(0,I_256).
```

For any degree-one homogeneous network output `F`, define

```text
G F(u) = E_Z F(Y) / c,
c = E ||rho*U + tau*Z||,  U uniform on S^255.
```

The spherical integral of `G F` equals that of `F` exactly.  The Gaussian-
scale prediction is `E[R] * Q_K129 G F`, where `R ~ chi_256`.

`c` is evaluated analytically with the noncentral-chi mean identity and must
agree with a fixed-seed five-million-sample scalar Monte Carlo check within
four standard errors before capture.

Each replicate uses one independent Gaussian vector per unoriented K129 line
and evaluates the four-point antithetic set

```text
rho*u + tau*z, rho*u - tau*z,
-rho*u + tau*z, -rho*u - tau*z.
```

This is unbiased for the two centers `+u,-u`.  There are 64 independent
antithetic replicates.  Predictions are retained at layers
`1,2,4,8,16,24,32` so a later closure hierarchy can be compared without
rerunning the packet truth.

## Fresh rows and seal

```text
Full1000:    640..647
Generated128: 88..95
base seed:   2026080711
```

Repository search found no named prior use of these rows in the current
capsule.  The capture process may read weights only and must seal predictions
before the scorer opens targets.  The rows are confirmation rows; epsilon,
normalization, noise coupling, and gates may not change after scoring.

## Estimator and gates

The scorer uses the same unbiased cross-replicate U-statistic as R1 to remove
finite-replicate conditional Monte Carlo variance.  All gates are required:

- pooled packet/q0 unbiased raw-MSE ratio `<=0.60` (at least 40% reduction);
- Full ratio `<=0.70`;
- Generated ratio `<=0.70`;
- each 32-replicate-half pooled ratio `<=0.70`;
- at least 12 of 16 rows improve;
- 20,000-row-bootstrap probability that the pooled ratio is `<=0.60` is at
  least `0.90`;
- the 64-replicate plug-in ratio is `<=0.70`, ensuring the claimed effect is
  visible rather than wholly dependent on a noisy U-statistic correction.

A pass authorizes only Stage C: an overpowered full-covariance packet moment
closure compared layer-by-layer with this sealed truth.  It does not authorize
a block closure, estimator change, package, score projection, upload, or
submission.

