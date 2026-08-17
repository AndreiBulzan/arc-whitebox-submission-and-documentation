# Verdict: K129 diagonal depth-closure MLMC R1

Date: 2026-08-09

Verdict: **kill the diagonal-closure depth telescope**.

Evidence label: **component plus projection**.  The target-free capture used
four Full and four Generated development weights and no targets.  It opened
no FlopScope session, physical row, Mini100 row, package, upload, submission,
or remote action.

The exact finite-population identity passed with maximum coordinate error
`1.12e-8`.  Therefore the failure is statistical/economic, not an algebra or
implementation failure.

Using all 129 complete bases, every possible interval `a -> b`, and integer
basis allocations under a conservative 27.2B projected cap, the optimal
target-free allocation on Full, Generated, and pooled variances was the same:

```text
path:          0 -> 32
bases:         25
projected C:   27.05B
extra MSE:     1.2083e-6 Full
               1.1728e-6 Generated
               1.1905e-6 pooled
```

In other words, the optimizer discards every diagonal-closure control and
chooses ordinary complete-basis sampling.  Its extra variance alone exceeds
the `1e-6` raw target before the finite K129 residual is added.  No target
access is needed to kill this spelling.

This does not close a cheap covariance-enriched control.  A
diagonal-plus-low-rank closure is the only licensed immediate successor;
literal full covariance is approximately as expensive per tail layer as
propagating the complete 512-row basis and therefore cannot create a score
floor advantage.

Controlling artifact:
`runtime/artifacts/k129_depth_closure_mlmc_r1_targetfree_20260809.npz`.
