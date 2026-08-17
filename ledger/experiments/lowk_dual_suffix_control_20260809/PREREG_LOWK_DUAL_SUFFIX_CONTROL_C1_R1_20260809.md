# Low-K dual suffix control C1 R1 preregistration

Date: 2026-08-09

Evidence sought: target-free CUDA **component** capture on the fixed sixteen-row
public development pilot, followed by a post-seal accuracy diagnostic.  Any
operation count or adjusted score is a **projection** until a capsule-native
FlopScope 0.10 / WhestBench 0.14 graph has a measured-whole receipt.  This run
does not authorize an official Mini100 capture, package, upload, or submission.

## Motivation and target ceiling

An independently reported exact-package Mini100 estimator using twenty ordinary
and twenty polar complete Kerdock bases has raw MSE `1.057396850541e-6` at
`56.893444471B` effective compute.  This is external context, not capsule
evidence.  Its new observation is a target-free joint coreset chosen after
layer 5.

The benchmark floor is `27.2B`.  At that floor:

- raw MSE must be at most `1.0e-6` to score at most `1.0e-7`;
- raw MSE must be at most `8.0e-7` to meet the terminal `8.0e-8` objective.

Pure deletion from forty to twenty tails cannot reach the floor in the external
engine even with a zero-cost prefix: half its `54.722661632B` analytical work is
already `27.361330816B`, before residual.  A production descendant therefore
needs about twelve to fourteen exact suffix units, a cheaper full-population
suffix surrogate, or another exact arithmetic saving.

## Blocking prior-art preflight

Queries covered `multifidelity`, `orientation difference`, `L8 response`,
`late basis tail`, `mean path`, `shared suffix`, `midpoint suffix`, `late state
coreset`, `static coreset`, `basis pruning`, `polar`, and `control variate`.

Nearest controlled negatives:

1. `multifidelity_l8_response_20260729` used an exact layer-8 affine response
   across eight orientations and exact residuals in two selected orientations;
   it worsened both held families.  Its controlling failure was that the affine
   checkpoint response did not predict the orientation-mean nonlinear residual.
2. `late_basis_tail_telescope_20260808` killed fixed mean-path and diagonal
   Gaussian absolute suffix closures at checkpoints 16--31.
3. `k129_dual_midpoint_suffix_20260805` killed propagation of the arithmetic
   midpoint of two complete clouds through one suffix.
4. `k129_late_state_coreset_20260801` and the official-Mini static coreset
   killed direct particle/state deletion and merging.
5. The existing low-K portfolio established useful ordinary/polar
   decorrelation, but its deployable sampler-only curve does not bend the score
   curve.

The present observable is **materially new in the capsule but narrowly so**:
for each complete low-K basis, it propagates the positive and negative
checkpoint *basis means* through the actual nonlinear supplied-weight suffix.
It then uses the resulting per-basis output as a coarse model and purchases
exact suffix residuals for a small balanced subset.  It neither averages the
two frame states together nor uses an affine checkpoint map.  This explicitly
tests whether the richer nonlinear coarse model invalidates the L8-response
negative.  Failure closes this spelling.

## Frozen development population

- fixed sixteen public-weight rows and hashes inherited from
  `k129_pair_residual_sparse_pilot_20260730`;
- ordinary support: the frozen target-free nested O0 support, first 20 bases;
- polar support: the frozen O1 nested support, first 20 bases;
- checkpoint: post-ReLU layer 5 (zero-based supplied-weight indexing);
- fine population: forty complete basis units, twenty per arm;
- coarse population: for each basis, the mean of its 256 positive rows and the
  mean of its 256 negative rows, propagated separately through the actual
  remaining weights and the same width screens;
- endpoint lambda `0.0075`, output scale `1.000025`.

The target-free candidate for an arm with population `U` and fine subset `S`
is

```text
mu_hat_arm = mean_{i in U}(g_i) + mean_{i in S}(f_i - g_i),
```

and the two arm results are averaged equally.  Here `f_i` is the exact
complete-basis suffix endpoint and `g_i` is the two-row nonlinear mean-path
endpoint.  The primary subsets are selected independently within each arm by
greedy centroid matching of standardized layer-5 paired first and diagonal
second moments.  Frozen subset sizes are `m={4,6,7,8,10}` per arm, corresponding
to `8,12,14,16,20` exact tails total.  Fixed-prefix subsets are recorded as a
guard.

The capture may compute all forty fine endpoints to expose the component
oracle.  A production graph would compute only the selected fine tails.

## Post-seal gates

Primary promotion requires one fixed target-free cell with all of:

1. total exact tails at most 14;
2. pooled raw MSE at most `1.0e-6` on the sixteen-row development pilot;
3. raw MSE at most `1.2` times the exact forty-tail dual control;
4. improvement over the direct selected-tail estimator;
5. coarse/fine residual correlation positive in both ordinary and polar arms;
6. an optimistic static count projection at or below `27.2B`.

The terminal-quality capacity marker is raw MSE at most `8.0e-7`.  Passing the
development gate authorizes only a frozen official-Mini100 implementation.
Failure at every `<=14`-tail cell kills nonlinear basis-mean suffix control on
this geometry.  We will not tune coefficients after targets open; the
coefficient is fixed at one by the multifidelity identity.

