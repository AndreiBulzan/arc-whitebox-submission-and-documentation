# Coupled zonal-H2 residual cloud R1 — verdict

Date: 2026-07-29

Evidence label: **component**. The capture opened only weights from the
already-used Full rows `{0,1}`, Generated rows `{0,1}`, and the sealed
target-free direct-K24 arrays. The new predictions were frozen to an NPZ
with SHA-256
`2fa516d6d7d8078a480e9eb139d46eeb6443ecd9782d36d3a918c13d1e5f00ba`
before a separate scoring process opened the four development targets.
There was no FlopScope session, physical/timed run, package, upload,
submission, or remote action.

## Decision

**Kill this exact one-axis zonal-H2 recurrence.**

It is a genuine positive two-family signal, but it does not pass the
preregistered remotely relevant gate and is not stable row by row:

| K24 final | direct H1 MSE | zonal-H2 MSE | candidate/direct |
|---|---:|---:|---:|
| Full2 | `1.129845004e-5` | `8.242984335e-6` | `0.729568` |
| Generated2 | `1.858972301e-6` | `1.529029735e-6` | `0.822513` |

The correction removed `27.04%` of pooled Full MSE and `17.75%` of pooled
Generated MSE. That is materially stronger and more transferable than the
killed first-harmonic recurrence on Generated, but the frozen keep gate
required ratio `<=0.50` and candidate MSE `<=1.2e-6` on each family. It
passed neither accuracy condition.

The pooled improvements also conceal within-family reversals:

| row | direct MSE | zonal-H2 MSE | ratio |
|---|---:|---:|---:|
| Full 0 | `2.146298042e-5` | `1.394483724e-5` | `0.64972` |
| Full 1 | `1.133919661e-6` | `2.541131426e-6` | `2.24102` |
| Generated 0 | `2.803876595e-6` | `1.852743888e-6` | `0.66078` |
| Generated 1 | `9.140680068e-7` | `1.205315582e-6` | `1.31863` |

Thus the result is not a candidate for broad acquisition or production
engineering. Per the preregistration, there is no K, coefficient, clipping,
axis, or layer-schedule follow-up.

## What was tested

For every live basis/output, the antipodal odd trace defines a unique linear
harmonic `r x`. The new observable projected the even trace onto the
rank-one degree-two harmonic

```text
h2(x) = x² - 1/256
```

with its target-free least-squares coefficient `c`. The surrogate

```text
g(x) = m + r x + c h2(x)
```

has exact spherical ReLU first/second moments: its positive set is at most
two intervals, and the implementation integrates the resulting degree-four
polynomials with incomplete-beta moments. The literal cloud carried

```text
exact sphere integral of ReLU(g)
  + sampled [ReLU(z) - ReLU(g)]
```

at every layer and was moment-matched before the next matrix. This is a
real layerwise higher-harmonic coupling, not the killed marginal Gaussian
or independent full-covariance control variate.

The unit gate passed exactly:

```text
linear surrogate max error                 0
linear H2 coefficient max                  0
affine-sphere first-moment max error        1.63e-16
affine-sphere second-moment max error       1.39e-17
quadratic-vs-dense first-moment max error   2.78e-17
quadratic-vs-dense second-moment max error  3.47e-18
```

All K4/K24 trajectories were finite. At layer 31 the one-axis H2 projection
explained only `4.26%--5.66%` of the live even-trace energy. This is the
mechanistic boundary: a single zonal axis sees real transferable signal, but
most of the basis-varying even state is multi-axis or cross-basis connected.

## Economics

Evidence label: **projection**, not a FlopScope receipt.

Separating a fixed `1.31B` closure from the current K162 steady count and
scaling cloud work by `24/162` gives:

```text
K24 propagated graph projection             24.753301624B
deliberately conservative H2 ceiling        16.000000000B
total current-0.9.1 count projection        40.753301624B
```

The `<60B` count gate passes. Accuracy and rowwise stability kill the method.
There is no wall, residual, effective-compute, result-cap, or runnable
estimator claim.

## Exact information retained

The first transferable higher-harmonic signal is real: an aligned rank-one
H2 component improved the pooled error in both named families, unlike the
earlier marginal K3/K4 recurrence. But the axis explains only about five
percent of late even energy and reverses on one of two rows in each family.
A lawful reopen needs a genuinely multi-axis or cross-basis connected state
that captures substantially more even energy at comparable cost. Retuning
this scalar recurrence does not qualify.

## Frozen artifacts

```text
preregistration
048ce67d498b3d7563bdcf44b564cb95b0d84205b9095c523e3c61d23844528a

source
140290dee0360f6c0c90ac0e87dffa82126e0e3d4f30cae9fc72e3824e9ee5e9

unit receipt
e504a37008a2110458d3408148f027e2e11555b1f57eac5542e433aec1ec9cc4

K24 target-free predictions
2fa516d6d7d8078a480e9eb139d46eeb6443ecd9782d36d3a918c13d1e5f00ba

K24 target-free manifest
3d3cd5bafcb89a88ad05df5883bd05a804c0cffc81f80cace06e1f464927ce6c

K24 post-seal component score
c11d48e2b9f157576c7d7583f6774e8e3710b4510c576febf24a18ebc30fba50
```
