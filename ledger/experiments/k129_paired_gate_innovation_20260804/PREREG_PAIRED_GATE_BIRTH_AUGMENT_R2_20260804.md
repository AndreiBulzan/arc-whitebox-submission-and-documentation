# Paired gate-birth augmentation R2 preregistration

Date: 2026-08-04

Evidence scope: **component**.  This is a target-free Full16/Generated16
teacher-reconstruction screen.  It may not open challenge targets, start a
FlopScope session, build a package, upload, or submit.

## Prior-work boundary

The capsule has already killed:

- scalar and sparse selections of partial complete frames;
- a mean/variance tangent continuation of the mixed arm;
- a noisy 26-particle layerwise gate-innovation accumulator; and
- endpoint, q0-D6, dispersion, and variance distillations of the R36 arm.

R2 does not repeat those constructions.  It uses the deterministic paired
particles which R36 already propagates through layers 2--5.  At each paid
ReLU site it separates the alternate-minus-q0 difference into

```text
delta_h = 1[z_q0 > 0] * delta_z + birth,
birth   = ReLU(z_alt) - ReLU(z_q0) - 1[z_q0 > 0] * (z_alt - z_q0).
```

The per-basis mean of `birth` is transported to the final output through the
realised mean-gated downstream Jacobian.  The existing 48 alternate atoms
and their matched q0 atoms are already live in the R36 graph.  After fixed
per-layer/per-atom coefficients are applied, only one 256-vector per paid
layer needs downstream transport.  This is therefore a cheap nonlinear
receiver, not another alternate-frame propagation.

## Frozen pilot

- Rows: first 16 rows of the target-free Full and Generated frame banks.
- Baseline: sealed R29 mixed48/D6 target-free prediction.
- Teacher: sealed equal average of q0, right, d2, and d2right complete-frame
  endpoints.
- Features: transported paired gate births from layers 2, 3, 4, and 5.
- Fits: train on one family and score only on the other, then reverse.
- Prefixes: layers `2`, `2--3`, `2--4`, and `2--5`.
- Relative ridges: `1e-4, 1e-3, 1e-2, 1e-1, 1, 10`.

Promote only if one prefix gives teacher-reconstruction MSE no worse than
`0.95` times R29 in both transfer directions.  A ratio above `1.0` in either
family kills this spelling immediately.  No challenge target is part of the
decision.

