# Verdict: low-rank depth-closure MLMC R2

Date: 2026-08-09

Verdict: **kill ranks 4, 8, and 16, and close the budget-compatible depth
telescope family**.

Evidence label: **component plus projection**.  Four Full and four Generated
weight rows were captured target-free.  No FlopScope session, physical row,
Mini100 row, package, upload, submission, or remote action was opened.

For each basis and depth, R2 fitted the exact top-r covariance eigenspace,
retained an exact diagonal residual, and transported it through the literal
tail weights.  This is an oracle advantage over a production randomized
range fit.  All ranks telescoped to the exact basis endpoint with maximum
coordinate error `5.59e-9`.

The decisive result is invariant to rank and even to price.  Granting the
low-rank closure, covariance extraction, and all tail transport **zero
cost**, the optimal 27.2B allocation on Full, Generated, and pooled
target-free variances is still:

```text
path:         0 -> 32
bases:        25
extra MSE:    1.2083e-6 Full
              1.1728e-6 Generated
              1.1905e-6 pooled
```

No intermediate depth is selected.  Conservative closure pricing produces
the identical result.  Because exact top-r factors fail even for free, no
cheaper randomized rank-r implementation at `r <= 16` can pass.

A literal full covariance is not a score-floor escape: each covariance
sandwich costs approximately as much as propagating one complete 512-row
basis through that layer.  The representation might reduce variance, but it
removes the compute advantage the telescope needs.  Reopen only with a
qualitatively cheaper sufficient statistic, not a wider covariance factor.

Controlling artifact:
`runtime/artifacts/k129_lowrank_depth_closure_mlmc_r2_targetfree_20260809.npz`.
