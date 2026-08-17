# Low-K transversal Rao--Blackwell T1/R1 preregistration

Date: 2026-08-09

## Question and evidence boundary

Can the exact finite-normal-sample Rao--Blackwell transform of the already
lawful K12 final preactivation cloud recover enough of the unavailable
rectified-normal teacher to yield raw MSE at most `9e-7`, preferably `8e-7`,
while remaining inside the `27.2B` compute floor?

This is a **component development diagnostic** on the existing aligned
256-network public offline-training bank.  The bank and its `160/48/48`
split have been used by earlier work and are not a virgin challenge family.
No official Mini100 row, FlopScope physical row, package, upload, submission,
or remote action is authorized by this experiment.

## T0 teacher clarification

The unavailable teacher's mean and variance come from public `K=1e6`
Gaussian-input moment passes.  `gaussian_exact` is the rectified-normal
functional of those high-sample preactivation moments.  TRB therefore targets
case 1 in the expert report: a Gaussian-input high-sample moment teacher, not
the finite K129 basis mean.

The frozen K12 state retained four preactivation moments per basis but not the
individual `12 x 512 x 256` final preactivations.  T1 first repeats the exact
hash-pinned K12 CUDA propagation and stores that missing target-free array.
It must reproduce the existing K12 per-basis moments and direct prediction
before any transform is scored.

## Blocking prior-art and target-ceiling preflight

Queries covered `K12`, `Rao Blackwell`, `UMVU`, `finite sample`, `transversal`,
`permutation bank`, `Gaussian ReLU`, `rectified normal`, `jackknife`,
`U-statistic`, `cross-basis`, `analytic shrink`, and the literal K12 capture
call site.

Nearest controlled negatives are:

- global plug-in and James--Stein first/second-moment shrinkage;
- cross-basis unbiased-square and exhaustive jackknife formulas;
- hidden-covariance spectral shrinkage;
- learned pair-teacher students and K3/K4 feature bridges.

TRB is a **materially new finite-sample observable**.  It retains every final
preactivation, rewrites the direct endpoint as exactly balanced one-row-per-
basis experiments, and conditions each experiment before averaging.  None of
the prior formulas preserved this rowwise transversal structure.

The ceiling passes conditionally.  The K12 unavailable-teacher blend has
held raw MSE about `7.81372e-7`.  The transform adds a projected
`0.03--0.12B`, leaving a K12 or K24 implementation within the score floor if
the statistical gate passes.  Count remains a projection until a whole
FlopScope receipt exists.

## Frozen target-free capture and transforms

- Support: the exact existing nested K12 support.
- Final state: unshifted signed preactivations in basis-major
  `(12, 512, 256)` order.
- Endpoint shift, sphere multiplier, and output scale: the literal incumbent
  values `lambda=0.0075`, `chi_mean(256)`, and `1.000025`.
- Ordinary permutation seed: `202608091917`.
- Independent permutation banks: eight signed-row banks over 512 rows and
  eight antipodal-pair banks over 256 directions.  Every basis permutation is
  uniform and uses every row/direction exactly once.

Before opening the teacher or target, save:

1. direct K12 endpoint reconstructed from the captured preactivations;
2. global plug-in, exact K129 finite-population variance correction, and its
   delete-one jackknife;
3. full-cloud normal UMVU diagnostic with sample size `6144`;
4. signed-row TRB (`n=12`) with `L in {1,2,4,8}`;
5. antipodal-pair TRB (`n=24`) with `L in {1,2,4,8}`;
6. disjoint first-four/last-four bank averages for both TRB units.

The normal conditional statistic uses

`a = sqrt((n-1) * Q / n)`

and the exact even-`n` polynomial.  Candidate generation must contain no
challenge target, `gaussian_exact`, or final expectation.

## Post-seal protocol

Open `gaussian_exact` and `target_h31` only after the complete candidate file
and its hash are frozen.

- Fit one global scalar `beta` for each candidate on the first 160 networks,
  clipped to `[0, 1.5]`, in `direct + beta * (candidate-direct)`.
- Select one candidate by raw MSE on the next 48 development networks.
- Report the final 48 held networks once without retuning.
- Separately report target-aware best scalar capacity on held, correction
  cosine with `gaussian_exact-direct`, MSE to `gaussian_exact`, and the
  first-four/last-four bank disagreement.
- Recompute the original unavailable-teacher blend under the identical split
  as the reference gain denominator.

## Gates

Retire TRB if any of these holds:

1. best target-aware held scalar capacity retains under `70%` of the original
   direct-to-unavailable-teacher-blend gain;
2. the selected transform's pooled correction cosine with the unavailable
   teacher correction is below `0.25`;
3. for its effective unit, the L8 first-four/last-four disagreement MSE is
   more than half the L8 correction energy relative to direct;
4. the frozen selected held raw MSE exceeds `9.0e-7`.

Held raw MSE at most `8.0e-7` is the breakthrough gate.  A pass authorizes a
production-shaped K12/K24 implementation and exact official Mini100 ladder;
it is not itself Mini100 or a score receipt.

