# Schur-harmonic discrepancy balance R1

Date: 2026-08-05

Evidence sought: target-free **component** capture on fixed Full16 and
Generated16 rows, followed by a post-seal split-half score. No FlopScope,
package, Mini100, upload, or submission action is licensed by this screen.

## Prior-work boundary

The capsule already killed Euclidean/polar coefficient grids, geodesic
midpoints, incomplete-basis mixtures, whole-basis interleavings, and
energy-only Schur partitions. The surviving lead is the fixed complementary
angle-alternating Schur frame, which scored raw ratio `0.96819` on official
Mini100 at the same trajectory count. No prior artifact balances the signed
Schur rotation harmonics themselves.

## Fixed construction

For the relative rotation from q0 to `polar(q0 + right + d2)`, use its real
Schur one/two-dimensional invariant blocks. Each block has signed angle
`theta`. Greedily two-colour blocks, largest feature norm first, to minimize
the discrepancy between the two colours in the fixed Fourier feature sets

```text
h2: [dimension, cos(theta), sin(theta), cos(2 theta), sin(2 theta)]
h4: h2 plus harmonics 3 and 4
h6: h4 plus harmonics 5 and 6
```

The feature columns are normalized before colouring. Both complementary
complete frames are retained. Two controls are frozen: the existing
angle-alternating pair and a four-harmonic balance weighted by the
first-layer energy in each Schur block. No targets enter frame construction.

Rows are fixed before capture:

```text
Full       320..335
Generated   80..95
selection   even positions within each family
validation  odd positions within each family
```

The primary is chosen using only the selection halves by minimizing the
worst Full/Generated pooled raw-MSE ratio. Promotion requires that unchanged
primary on both validation families has ratio <= 0.96, pooled validation
ratio <= 0.94, and at least 18/32 rows improve overall. Passing licenses one
unchanged official-Mini100 capture; it does not license estimator changes or
physical deployment.

