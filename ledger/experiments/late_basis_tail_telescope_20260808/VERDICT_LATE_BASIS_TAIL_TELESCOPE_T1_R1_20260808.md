# Verdict: reject the tested late per-basis tail telescope

Date: 2026-08-08

## Outcome

Reject fixed mean-path and diagonal-Gaussian per-basis closures at checkpoints
16, 20, 24, 28, 30, and 31.  This is a narrow representation kill.  It does
not reject every multilevel or control-variate construction.

The experiment used exact K129 per-basis states on fixed Full640--641 and
Generated88--89 rows, no target values, and no FlopScope or physical runs.
For each checkpoint, the remaining tail was closed independently for all 129
bases and exact residuals were replayed for sample sizes 1 through 64.

The best arm was already the easiest case: diagonal-Gaussian closure after
layer 31 with 64 of 129 exact basis tails.  Its pooled added MSE was
`1.31118e-8`.  Even under optimistic projected compute accounting, the score
was `1.18409e-7`, or `4.42%` worse than the R89 reference.  Both families
worsened and the preregistered gates failed.

Evidence label: **component** for estimator error and **projection** for cost
and score.

Evidence:

- `runtime/artifacts/late_basis_tail_telescope_t1_r1_targetfree_20260808.json`
- `runtime/artifacts/late_basis_tail_telescope_t1_r1_targetfree_20260808.npz`

