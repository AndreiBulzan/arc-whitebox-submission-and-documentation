# Preregistration: high-precision dense and polar arc-cosine teacher R2

Date: 2026-08-09

R1's 65,536-row synthetic teacher was too noisy at the scale of the basis
correction: it achieved only 14% post-seal pooled reduction and reversed on
Generated, while IID pilots through 4,096 rows were neutral.  R2 resolves
whether that is a representation failure or target-feature sampling noise.

For the exact same sealed candidate features and synthetic final rows, R2
computes:

- a 4,194,304-row generic antithetic target-feature mean in 65,536-row
  streaming batches;
- two independent sequences of complete Haar orthogonal sphere frames, with
  cumulative 1, 2, 4 and 8 frames (512--4,096 rows).

The dense teacher is a high-precision population-capacity test.  The Haar
frames are the production-shaped low-harmonic quadrature test.  Both evaluate
the actual supplied first 31 layers; no deep kernel continuation is used.

This capture opens weights only.  It opens no benchmark targets, FlopScope
session, physical row, Mini100 row, package, upload, submission, or remote
action.

Gates remain:

- high-precision dense teacher: at least 35% pooled post-seal raw reduction
  and no family below 25%;
- at most eight Haar frames: at least 20% on both families and at least 70%
  of dense gain;
- if dense fails, kill the population teacher; if dense passes but polar
  fails, record a sample-complexity kill at the production budget.

