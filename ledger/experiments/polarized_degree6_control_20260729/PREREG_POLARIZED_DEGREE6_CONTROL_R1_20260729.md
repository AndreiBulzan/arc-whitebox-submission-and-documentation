# Polarized degree-6 regression cubature R1

Date: 2026-07-29

Evidence sought: **component**. This is a target-free capture followed by a
separate signed-final-preactivation score on 16 already-open Full and 16
process-separated Generated rows. It authorizes no FlopScope session,
physical or timed row, package, upload, submission, or remote action.

## Mechanism

The complete Kerdock orientation is an exact spherical 5-design, so its first
unresolved angular component has degree 6. For every unit direction `v`,
define the dimension-256 zonal spherical harmonic

```text
h_v(x) =
  d^3 * [
    t^6
    - 15 t^4/(d+8)
    + 45 t^2/((d+6)(d+8))
    - 15/((d+4)(d+6)(d+8))
  ],
  t = x dot v, d = 256.
```

Its exact uniform-sphere mean is zero. Therefore its per-basis mean is a
lawful known-mean control feature.

For each MLP, choose the eight first-layer gate normals with largest

```text
||W0[:,i]|| * ||mean-gated downstream response from neuron i||.
```

For ranks `r in {4,6,8}`, form a polarized feature set containing the first
`r` normals and every normalized pairwise sum and difference. This gives
`r^2` degree-6 features and includes mixed six-way input-coordinate
contractions that a collection of marginal `v^6` probes does not isolate.

Fit the paid orientation-0 per-basis signed endpoints to these known-mean
features with an unpenalized intercept. Select `r` and ridge strength from

```text
r in {4,6,8}
alpha in {0.01, 0.1, 1.0}
```

by target-free leave-one-basis-out generalized cross-validation. Apply the
same fitted coefficient to the literal paid `129 + m` rule, for
`m in {17,33}`:

```text
candidate = literal_basis_mean - mean_paid_features @ beta.
```

The primary method is this GCV-selected polarized rule. Fixed-rank results
and an individual-zonal response-weighted diagonal control are diagnostics
only.

## Why this is distinct

- It does not route or replace supports.
- It does not fit an endpoint correction to challenge targets.
- It does not use scalar marginal moments, a Gaussian mixture, a Stein
  identity, an L8 multifidelity residual, or the killed degree-6 linear
  adjoint support objective.
- It observes the actual nonlinear endpoint variation across every already
  paid basis and uses a cross-coordinate degree-6 function class whose exact
  population mean is known.

The prior basis gate-cell control used four nearly constant ReLU pilot
features on 8--16 basis replicates. R1 instead has 129 orientation-0
replicates and feature variation exactly at the first unresolved design
degree.

## Screen and gate

Rows are `0..15` in each family. Freeze all predictions before the scorer
opens `target_final_premean.npy`.

The primary GCV rule is promotable to a broader cached screen only if, for
at least one of `m=17` or `m=33`, both families independently satisfy:

```text
pooled signed MSE / literal pooled signed MSE <= 0.80
row-ratio p95                              <= 1.50
rows improved                              >= 60%
all predictions finite
```

Failure kills this exact first-layer polarized degree-6 regression-cubature
spelling. It does not close higher-order controls based on genuinely late
gate normals or a different exact representation.

## Current-meter economics

This screen makes no physical-price claim. A direct dense implementation of
the selected `r=8` feature ceiling evaluates 64 harmonics over at most 162
bases and 256 vectors per basis, below roughly `5.5B` ordinary multiply/add
operations if dot products are counted as `2*d-1`. Rank 4 is below roughly
`1.4B`. Endpoint reductions and the small ridge solve are negligible by
comparison. These are **projections**, not FlopScope 0.9.1 receipts, and any
passing rule would require a current-meter static ledger before estimator
work.
