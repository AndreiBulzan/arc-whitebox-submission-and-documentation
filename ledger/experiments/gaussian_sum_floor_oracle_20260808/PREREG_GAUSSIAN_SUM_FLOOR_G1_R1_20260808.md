# Gaussian-sum score-floor G1/R1 preregistration

Date: 2026-08-08

Evidence scope: target-free offline **component** capture followed by a
separate post-seal broad-development score.  This is not a FlopScope run, a
whole-estimator receipt, a production-price receipt, a Mini100 gate, a
package, an upload, or a remote result.

## Question

Can a state-only, full-covariance Gaussian mixture fitted at a deep
**preactivation** boundary, followed by a positive local spherical-radial
cubature and an exact supplied-weight tail, reach the score-floor accuracy
regime?

The benchmark scores each row as

```text
raw_mse * max(0.1, effective_compute / 272B).
```

Consequently a separately implemented estimator at or below `27.2B` needs
raw final MSE below `1.0e-6` to score below `1.0e-7`.  The teacher-capacity
continuation threshold is deliberately stronger: `7.5e-7` pooled and no
family above `1.0e-6`.

## Prior-art preflight

Capsule searches covered `Gaussian sum`, `Gaussian mixture`, `split-merge`,
`sigma point`, `spherical-radial`, `cubature Kalman`, `full covariance`, and
`exact tail handoff`, plus the `256 x 256` covariance and late-tail call
sites.

Nearest controlled negatives:

- `anchored_tail_gaussian_20260808`: one Gaussian fitted to the complete
  K129 activation cloud failed even with only two approximate late layers;
- `response_rank8_covariance_20260729`: sixteen conditional components were
  propagated by Gaussian closure through the whole depth and failed;
- `prefix_telescoping_control_20260729`: a shallow exact prefix plus a
  collapsed suffix failed;
- PTCC conditional mixtures treated a selected signed quadrature cloud as a
  probability law and failed transfer.

This experiment is `materially new observable/execution surface`: it fits a
global full-rank mixture to an independent Gaussian-input teacher at a deep
preactivation boundary and performs no closure after handoff.  Failure of a
single fit spelling will not be promoted into a theorem about all mixtures.

## Fixed development rows and data discipline

Use already-open development rows only:

```text
Full       640, 641
Generated  88, 89
```

The target-free stage may read weights but not expectation targets.  It uses
independent `N(0,I_256)` inputs generated from frozen seeds.  Mixture fitting
uses preactivation states only; it may not use final activations, expectation
targets, residuals, or row losses.  Predictions and hashes are sealed before
the scoring stage opens the already-used targets.

The primary handoff is layer 24 preactivation: propagate the Gaussian input
through ReLU layer 23, compute `z24`, fit the mixture to `z24`, apply the
layer-24 ReLU exactly to every handoff point, and propagate layers 25--32
exactly.

## Mixture fits

Primary component counts:

```text
K = 1, 4, 8, 12, 20
```

Use deterministic state-only MiniBatch K-means with at least two frozen
initialization seeds for `K>1`.  For each assigned component, record its
empirical weight, full float64 mean, and full float64 covariance.  Add only a
declared trace-scaled eigenvalue floor needed for factorization.  Record
minimum cluster size, covariance trace, effective rank, and applied ridge.

This is a teacher-capacity fit, not the proposed recursive split/merge
construction.  A pass licenses that later oracle; it does not license an
estimator.

## Two independent tail tests

### Mixture-distribution ceiling

Draw frozen, antithetic Gaussian samples from every fitted component and
propagate them through the exact tail.  Use multiple independent replicates
so numerical Monte Carlo uncertainty can be measured.  This estimates the
best tail mean represented by the fitted mixture without local-cubature
error.

### Positive local cubature

For component `N(mu, Sigma)` with `Sigma = L L^T`, use

```text
mu +/- sqrt(256) * L q_j
```

with equal positive weights.  Test:

```text
B = 1: identity basis
B = 2: identity plus one frozen orthogonal Hadamard/sign basis
```

Apply ReLU at the preactivation handoff and then the exact tail.  No Gaussian
closure is permitted after handoff.

## Cost ceiling

The production leading-count projection is

```text
2 * 256^3 * K * [t + B * (32 - t)].
```

At `t=24`, relevant points are approximately:

```text
K=20, B=1  21.475B
K=12, B=2  16.106B
K=8,  B=2  10.737B
```

These are projections only.  Factorizations, component construction,
special functions, movement, and residual time must later be metered.  No
configuration whose leading count exceeds `24B` can be promoted without a
separate convincing residual allowance.

## Gates and non-destructive interpretation

For each row, family, `K`, fit seed, and `B`, report final raw MSE.  Also
report random-tail replicate dispersion and cubature-versus-random-mixture
MSE.

Class-survival gate:

```text
pooled cubature raw MSE <= 7.5e-7
each-family cubature raw MSE <= 1.0e-6
mixture-random ceiling is not materially worse than cubature
```

Interpretation:

- If the random-mixture tail passes and cubature fails, only the tested
  local rule is rejected; test additional positive orientations before
  judging the mixture.
- If one fit seed fails, do not reject `K`; require seed stability or use a
  stronger state-only partition.
- The finite primary grid can establish survival.  It cannot kill every
  `K<=20` mixture unless an overpowered fit/partition and uncertainty audit
  also fail below `1.0e-6`.
- Recursive adaptive propagation is forbidden until this teacher-capacity
  gate passes.

## Integrity

- Hold `runtime/.benchmark_lane.lock` during GPU capture.
- Refuse output overwrite.
- Pin source, preregistration, weight archive, and output hashes.
- Record CUDA/torch versions and deterministic settings.
- No official Mini100, physical runner, FlopScope, package, upload, or
  submission action is authorized.

