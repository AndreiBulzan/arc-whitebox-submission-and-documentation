# Weight-graph reservoir R1 preregistration

Date: 2026-07-29

## Hypothesis

The remaining K146 final error may contain a deterministic, transferable
contraction of the complete realised weight graph which is absent from final
moment/readout features.  Test one fixed, weight-only,
hidden-permutation-equivariant graph reservoir over all 32 matrices, followed
by one shared linear residual head over output coordinates.

This is not a moment closure, a particle-trajectory response, a support
router, PTCC, or an extra quadrature cloud.  It never observes a hidden
activation.  The first-layer state is built from `W0.T @ W0`, so it respects
the input Gaussian's orthogonal invariance.  Later updates use only
permutation-equivariant contractions through `W`, `abs(W)`, and `W**2`, plus
source-permutation-invariant column statistics.

## Frozen screen

- Fixed reservoir seed: `2026072907`.
- Width: 24 channels.
- Nonlinearity: `tanh`.
- Channel normalization: coordinatewise centering and RMS normalization
  after every layer.
- Baseline inputs to the head: current prediction, its within-row centered
  value, and the 24 final reservoir channels.
- Head: standardized ridge with alpha in
  `{0.1, 1, 10, 100, 1000, 10000}`.
- Select alpha only on Full rows whose dataset index is `2 mod 4`.
- Fit rows are Full indices `0/1 mod 4`; final refit uses `0/1/2 mod 4`.
- Locked tests are Full indices `3 mod 4` and all Generated64 rows.
- Generated label-noise MSE is subtracted only from aggregate reported MSE,
  exactly as in the existing broad screens.

## Hard decision

`GO` requires corrected/baseline raw-MSE ratio at most `0.50` on both the
locked Full partition and Generated64, with at least 70% of whole MLP rows
improved in both.  Otherwise kill this exact graph-reservoir spelling.

No physical row, FlopScope session, package, upload, submission, or remote
action is authorized.

