# Preregistration: Gaussian-moment arc-cosine control R4

Date: 2026-08-09

## Reopen boundary

Prior global Gaussian output closures and Gaussian control variates failed as
standalone final-output estimators.  R2 supplies a materially new observable:
near-exact target means of 512 synthetic last-layer ReLUs identify shared
K129 basis weights that reduce corrected actual-output MSE by 92.63%.  R4 does
not reuse a Gaussian closure as the output.  It asks whether only the exact
mean and covariance of the penultimate state are sufficient to reproduce that
512-dimensional *teacher statistic*.

This is a strict ceiling test.  If a Gaussian with the near-exact target
layer-31 moments cannot provide useful calibration weights, no recursively
approximated single-Gaussian moment closure can do so.

## Fixed variants

On the same sealed four component rows and same 512 synthetic final directions:

1. stream 4,194,304 antithetic Gaussian inputs through the actual first 31
   layers and accumulate their target layer-31 mean and full covariance;
2. accumulate radialized mean and full covariance from the existing complete
   canonical K129 layer-31 states;
3. for each moment pair compute the exact rectified-normal marginal mean for
   every synthetic final direction;
4. repeat with covariance off-diagonals removed.

Fit the same sum-to-one ridge basis weights and seal every prediction before
opening the existing component targets.  The dense-moment variants are
teacher ceilings; the K129-moment variants are lawful production observables.

## Economics and gates

The K129 full covariance needs one `66048 x 256` Gram and small reductions,
roughly 8.7B conservative float32 arithmetic before solver costs.  Therefore
20% raw reduction is already interesting and 30% is a strong production lead.

- If the dense full-moment ceiling is below 20% raw reduction, kill every
  single-Gaussian moment bridge for this teacher.
- If dense moments pass but K129 moments fail, the missing object is accurate
  target moments rather than non-Gaussian shape.
- Promote a K129 variant only at at least 20% pooled reduction with neither
  family reversing.

No physical row, official Mini100 row, FlopScope session, package, upload,
submission, or remote action is authorized.
