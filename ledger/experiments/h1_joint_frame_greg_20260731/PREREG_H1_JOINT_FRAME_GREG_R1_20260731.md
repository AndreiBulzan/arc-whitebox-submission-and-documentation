# H1 joint-frame GREG R1 preregistration

Date: 2026-07-31

Evidence class: **component**. This is a fast public-development scout, not a
physical FlopScope row, broad statistical evidence, a package, or a remote
candidate.

## Question

The production O0 arm repairs every first-layer marginal mean and variance,
but it does not repair the off-diagonal first-layer second moments. For each
of the 129 complete Kerdock frames, form

```text
x_b = diag E_b[z_2 z_2^T]
```

after the literal production H1 affine repair. Its population value is known
exactly from the spherical arc-cosine kernel and the first two weight
matrices. Use that known mean to construct minimum-norm generalized
regression (GREG) weights over the 129 frame endpoint estimates.

This changes the cubature weights, not the propagated nodes. Antipodal
pairing and linear annihilation are preserved.

## Frozen screen

- Rows: the pre-existing sparse public pilot indices
  `0,1,17,63,127,255,511,1023,2047,4095,5000,6143,7167,8191,9001,9999`.
- Arm: complete O0 K129 arm only.
- Controls: standardized 256-vector `diag E[z_2^2]`.
- PCR ranks: `4, 8, 16, 32`.
- Damping: `0.5, 1.0`.
- Estimators:
  - full-sample minimum-norm GREG;
  - two-fold even/odd cross-fit GREG;
  - nonnegative-clipped full-sample weights as a stability diagnostic.
- Baseline: literal uniform mean of the 129 per-frame endpoint estimates.

No target is opened during capture. The fixed target-free capture is sealed
before the existing public pilot targets are opened by a separate scorer.

## Promotion / kill

This is only a directional pilot. Promote to a disjoint Full/Generated
two-family screen only if at least one **cross-fit** candidate satisfies all:

1. pooled MSE ratio `<= 0.92`;
2. at least `10/16` rows improve;
3. maximum per-row MSE ratio `<= 1.50`;
4. finite predictions and weights.

If no cross-fit candidate passes, kill H1 joint-frame GREG/reweighting in
this form. A full-sample-only win is reported but is not promotable.

