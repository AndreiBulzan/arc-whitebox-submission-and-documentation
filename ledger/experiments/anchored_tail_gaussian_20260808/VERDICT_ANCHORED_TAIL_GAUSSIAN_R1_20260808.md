# Anchored short-horizon Gaussian tail R1 verdict

Date: 2026-08-08

## Verdict

Kill exact-anchor Gaussian moment closure as compute barter, including the
overpowered full-bivariate covariance ceiling and the production
first-Hermite-plus-exact-diagonal recurrence.

This is target-free **component** evidence on fixed Full8 plus Generated8
rows.  No physical row, FlopScope session, Mini100 access, upload, submission,
or remote action occurred.

## Result

The exact canonical K129 particles were propagated to anchors
`24,26,28,29,30`.  Their empirical mean and full `256 x 256` covariance were
then used to replace only the remaining exact particle layers.

At anchor 30—only two approximate layers—the final Gaussian-scale
reproduction MSE was already:

```text
full bivariate ReLU covariance                 2.201480e-6
first Hermite + exact diagonal                 2.205622e-6
```

At the economically interesting anchor 28 it was:

```text
full bivariate                                 4.669619e-6
first Hermite + exact diagonal                 4.667430e-6
```

The current estimator's raw error is about `2.2e-7`; these closure errors are
one to two orders of magnitude too large.  Correlations near `0.99999` are
misleading because they are dominated by the large common activation mean.
The absolute signed mean error is fatal.

The result explains why a global closure and the successful local packet
closure are not contradictory.  A local packet starts Gaussian around one
centre.  The complete Kerdock activation law is a mixture of many separated
gate regimes; its first two moments do not determine the next ReLU mean even
over a two-layer horizon.

## Evidence

- `runtime/artifacts/anchored_tail_gaussian_r1_targetfree_20260808.json`
- `runtime/artifacts/anchored_tail_gaussian_r1_analysis_targetfree_20260808.json`
