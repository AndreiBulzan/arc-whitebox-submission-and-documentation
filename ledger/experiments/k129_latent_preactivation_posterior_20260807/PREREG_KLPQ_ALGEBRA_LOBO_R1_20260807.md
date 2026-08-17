# K129 latent-preactivation posterior quadrature — algebra/LOBO R1 preregistration

Date: 2026-08-07. This is a target-free component oracle. It opens no
expectation targets, changes no production estimator, performs no physical
FlopScope run, and performs no package, upload, or submission action.

## Hypothesis

Immediately before the final ReLU, retain the signed preactivation field on
all 66,048 canonical K129 nodes. Model each of its 256 output coordinates as
a zero-mean latent Gaussian process with normalized depth-31 ReLU-NNGP
correlation kernel. Condition that latent field, rather than the post-ReLU
activation, on the complete signed K129 observations and then take the exact
rectified-normal posterior mean.

The K129 antipodal MUB association scheme gives an exact inverse with five
projectors. This preserves all 66,048 observed coordinates; it is not a
low-rank state approximation. The first oracle tests only the inverse,
indexing, units, and leave-one-basis-out continuation. Stored rotated-frame
and dense-query tests are separately gated and are not licensed by a LOBO
pass alone.

## Prior-art preflight (blocking)

Queries searched in the capsule: `Bayesian quadrature`, `GP posterior`,
`latent preactivation`, `association scheme`, `MUB inverse`, `Kerdock
projector`, `negative preactivation`, `leave one basis out`, and signed-final
preactivation endpoint methods.

Nearest controlled work:

- the isotropic activation-GP Bayesian-quadrature no-headroom calculation,
  which conditions a Gaussian model for final activations and collapses to
  equal weights;
- signed-final-preactivation endpoint/control lanes, which retain basis
  means or fit target-free corrections but do not condition a latent GP on
  every signed node value;
- global/two-moment Gaussian closures, which recursively replace the full
  trajectory distribution and do not perform posterior interpolation of the
  observed final latent field;
- Kerdock projector and low-rank state experiments, which compress a state
  or tensor and are therefore not the exact five-eigenspace inverse proposed
  here.

Outcome: **materially new observable**. The invalidated assumption behind
the activation-BQ negative is that the observed post-ReLU field is itself
Gaussian. Here the GP is the latent preactivation and the nonlinear ReLU
posterior moment uses negative magnitudes that activation BQ discarded.

Target ceiling: the measured four-frame oracle reduces canonical raw MSE
from about `2.57e-7` to `9.1e-8`, so more than the required 35% oracle
headroom exists in off-design continuation. KLPQ strictly enriches the
post-activation observation with signed preactivations. This passes the
capacity preflight but is not an accuracy prediction.

## Frozen algebra

Dimension `d=256`, bases `B=129`, lines `M=Bd=33,024`, signed nodes
`N=66,048`. The normalized kernel is the ReLU correlation map composed 31
times. At inner products `(1,-1,0,+1/16,-1/16)`, the expected values are:

```
1
0.9720108731544704
0.9734181125699383
0.9736160100959503
0.9732406018244274
```

For the full design, the five expected eigenvalues and multiplicities are:

```
0.0251746480146      32895
0.0199556322063        128
64293.0127077             1
0.0219825945012      32768
0.796825266925          256
```

The implementation uses antipodal coordinates
`e=(z+ + z-)/sqrt(2)` and `o=(z+ - z-)/sqrt(2)`. The odd blocks are conjugated
by the 129 Kerdock basis matrices. The frozen compact artifact stores its
common orthogonal rotation in float32; its roughly `1e-8` orthogonality
roundoff is not part of the mathematical association scheme and is amplified
by inversion. For the projector algebra only, preregister a float64 polar
repair of that common rotation and rebuild all 129 bases from the unchanged
Kerdock phases. Report the maximum repair delta and all orthogonality/MUB
checks. The literal network propagation continues to use the unchanged
frozen float32 nodes. Zero-nugget inverse/Gram roundtrip
must interpolate every observed value with maximum float64 error below
`1e-8`; direct kernel queries on sampled observed nodes must independently
meet the same bound.

## Frozen data and holdouts

- Full rows `640..647`.
- Generated rows `88..95`.
- For every network, omit the same eight basis indices
  `{0, 1, 7, 19, 37, 64, 96, 128}` one at a time and condition on the other
  128 complete antipodal bases.
- No expectation target member is opened. Exact omitted-basis
  preactivations and activations are observations used only for this
  conditional-prediction falsifier.

The primary metric is MSE of the 256-dimensional omitted-basis activation
mean. The baseline is the activation mean over the 128 conditioning bases.
Pointwise preactivation and activation RMSE are secondary diagnostics.

## Posterior variance scale

The five-projector kernel is normalized to `k(1)=1`, while the unit-sphere
network preactivations have a smaller physical variance. Conditional means
are invariant to this amplitude, but rectified-normal posterior means are
not. Three frozen variants are reported:

1. `theory`: amplitude `2/256`;
2. `energy`: mean squared conditioning preactivation for each output;
3. `ml`: `z^T G^-1 z / N_train` for each output.

The preregistered primary is `ml`, clipped only for numerical safety to
`[0.25,4] * (2/256)`. The clipping count is reported. `theory` and `energy`
are diagnostics and cannot replace a failed primary after targets are seen.

## Gates

- **A0 algebra:** all five full-design eigenvalues match the frozen values
  within `2e-10` relative/absolute tolerance; analytic inverse/Gram
  interpolation and sampled direct-query interpolation are below `1e-8`.
- **A1 LOBO primary:** pooled held-basis-mean MSE reduction for `ml` is at
  least 30% relative to the other-128-bases mean.
- **A2 family guard:** reduction is at least 20% separately on Full8 and
  Generated8, and at least 12 of 16 networks improve after pooling their
  eight omitted bases.
- **A3 stability:** no nonfinite posterior, no negative posterior variance
  beyond `1e-12` numerical tolerance, and no more than 5% of output-scale
  estimates hit the safety clip.

Passing A0–A3 licenses the stored-rotation oracle. Failure of A0 is an
implementation error. The omitted MUB basis is a deliberately severe but
algebraically special query: its cross-kernel vector occupies only the global
even and common-coordinate odd projector sectors. Therefore A1/A2 failure
rejects the claim that LOBO itself validates KLPQ, but does **not** by itself
kill generic off-design continuation, whose kernel rows excite all five
sectors. With A0 green, exactly one preregistered stored-rotation oracle may
still run as the decisive generic-query test; no pilot, dense integration, or
production work is licensed unless that test reaches its separate 35% gate.

## Evidence and operations

The output is `component` evidence. The CUDA trajectory capture holds
`runtime/.benchmark_lane.lock`; its wall time is diagnostic and is not a
physical/effective-compute receipt. New files live in this directory and
`runtime/artifacts/`. The run does not read targets and cannot establish a
challenge score.
