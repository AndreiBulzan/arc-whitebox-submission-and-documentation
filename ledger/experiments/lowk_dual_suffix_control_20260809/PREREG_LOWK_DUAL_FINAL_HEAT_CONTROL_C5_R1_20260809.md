# Low-K dual final heat control C5 R1 preregistration

Date: 2026-08-09

## Question

Can the literal K20 ordinary plus K20 polar estimator recover at least a
small, score-relevant fraction of its remaining error by controlling only the
final ReLU boundary, using no extra network trajectories?

For the already-computed, lambda-corrected final preactivation cloud `z`, let
`m` and `v` be its coordinatewise empirical mean and variance.  For a
relative smoothing scale `a`, set `s = a sqrt(v)` and

```text
g_s(z) = s phi(z/s) + z Phi(z/s),
G_s    = ReLUNormalMean(m, sqrt(v + s^2)).
```

The candidate arm readout is

```text
baseline + alpha * (G_s - sample_mean(g_s(z))).
```

At `s=0`, this interpolates toward the matched-Gaussian closure.  As `s`
becomes large, it returns toward the empirical ReLU mean.  The correction is
target-free and uses only the final preactivations already required by the
incumbent statistic.

## Blocking prior-art and ceiling preflight

Capsule searches covered `Gaussian closure`, `final ReLU`, `smoothed ReLU`,
`heat`, `boundary`, `Edgeworth`, `Price/Wick`, and the literal
`gamma_readout` call site.  The nearest controlled negatives are:

- full-covariance Gaussian closure and anchored-tail Gaussian continuation,
  which replace or propagate the state distribution;
- endpoint lambda/Edgeworth grids, which shift final preactivation means or
  use raw marginal moments;
- Price/Wick boundary sources, which attempt to transport upstream connected
  innovations.

This attempt is a **materially new observable** in the capsule: the paired
finite-design discrepancy between a heat-smoothed final ReLU and its
matched-normal analytic integral, evaluated on the already-paid final cloud.
It neither propagates a Gaussian state nor transports an upstream source.

The target ceiling passes.  The independently banked K20+K20 Mini100 point
has raw MSE `1.057396850541e-6`; at the 0.1 floor it needs only `5.428%` raw
improvement to cross `1e-7`.  This experiment's transfer gate is 8%, and the
added network-propagation count is exactly zero.  Production transcendental
and movement cost must still be priced if the statistical gate passes.

## Frozen capture

- Evidence screen: the same fixed public16 component rows used by C1--C4.
- Arms: the frozen K20 ordinary and K20 polar supports from C2.
- Relative scales:
  `0, 0.125, 0.25, 0.5, 1, 2, 4, 8, 16`.
- Correction multipliers: `0.25, 0.5, 0.75, 1`.
- The capture opens no target and stores all candidate predictions before any
  score is inspected.
- The scale-zero/multiplier-zero baseline association is checked against the
  literal existing readout.

## Post-seal gate

The fixed row positions `0,2,...,14` are discovery and `1,3,...,15` are
validation.  Choose one common `(scale, alpha)` on discovery only, then report
it unchanged on validation.

Proceed only if all are true:

1. validation raw MSE improves by at least 8% versus the literal K20+K20
   baseline;
2. pooled raw MSE improves by at least 6%;
3. at least five of eight validation rows improve;
4. no validation row regresses by more than 25%.

Failure closes this final-boundary heat-control spelling only.  It does not
close deeper sparse-tail selection, a different exact arithmetic engine, or
the full low-K dual estimator.

## Evidence boundary

This is a component development screen, not broad statistical, Mini100,
physical, packaged, remote, or submission evidence.  A pass must be frozen
on disjoint data before an official Mini100 run.
