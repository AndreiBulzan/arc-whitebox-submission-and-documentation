# K129 adaptive-frame successor R1 — one candidate promoted to falsification

Date: 2026-07-29.

Evidence label: the geometry and count arithmetic are **component** and
**projection**, respectively. No GPU propagation, FlopScope session,
physical row, package, upload, remote action, or submission was performed in
this lane.

## Decision

Promote exactly one mathematically distinct same-K candidate to a fast
12-Full/12-Generated falsifier:

```text
frame                 H @ U.T,  W0 = U diag(sigma) V.T
O0 phase positions    1,5,9,...,125  (32 positions)
total support          K=129 unchanged
readout                exact R21 lambda=0.0075
projected count delta  about 0.256B; hard ceiling 0.270B
```

The candidate has a concrete mechanism absent from the killed right-SVD and
QR frames. Every substituted basis is orthogonal and therefore keeps exact
signed first and second moments. Moreover, the selected stored phases are
all bent, so `H D_p H` is flat and every row of the selected basis after
`W0` has exactly `||W0||_F^2 / 256` energy. It removes first-layer radial
leverage heterogeneity without changing K or fitting any target.

The just-completed fixed `W0.T @ W0` right-eigenframe candidate failed badly
(`1.145x` Full and `1.012x` Generated). That result does not close this
candidate: its frame was `V.T`, for which `V.T @ W0` is generally arbitrary
and no leverage-balancing identity follows. R1 instead uses the input-side
left singular frame and the exact bent-phase identity.

## Boundary

This is a credible, cheap falsifier—not an accuracy claim. The cross-frame
mixture could still damage higher-order mutual-unbiased structure, and only
the frozen final-prediction gate can decide that. If either family has pooled
ratio above `0.970`, stop; do not tune the 32 positions or frame mixture on
opened targets.

The exact immutable protocol is
`PREREG_K129_LEFT_SPECTRAL_BALANCED_FRAME_R1_20260729.md`.

