# Adjoint connected-innovation recurrence R1 — verdict

Date: 2026-07-29

Evidence label: **component**.  Cost is a **projection**.  The capture opened
only four weight rows and froze literal predictions before the separate
scorer opened two already-used Full and two already-used Generated targets.
No FlopScope session, physical row, package, upload, submission, or remote
action occurred.

## Decision

**Kill this exact rank-16 first-order recurrence as a checkpoint candidate.**

The new multi-site state is not numerically inert: it improved the
order-eight Price closure on every sealed row, by `42.5%` pooled on Full and
`68.8%` pooled on Generated.  But its absolute error remains two orders of
magnitude too large:

| family | Price8 raw MSE | R1 raw MSE | R1 / Price8 | rows improved |
|---|---:|---:|---:|---:|
| Full2 | `5.887443e-5` | `3.384669e-5` | `0.574896` | `2/2` |
| Generated2 | `7.016576e-5` | `2.191049e-5` | `0.312268` | `2/2` |

The preregistered continuation gate was `<=0.10` on each family.  Even at
the score multiplier floor of `0.1`, these raw errors imply approximately
`3.38e-6` Full and `2.19e-6` Generated adjusted, respectively `33.8x` and
`21.9x` above `1e-7`.  No rank, node count, layer window, coefficient,
clipping, or quadrature sweep follows.

## What was genuinely tested

R1 carries complete symmetric connected cores in the leading rank-16
downstream-response subspace:

```text
T3_l ~= Cum_3((h_l-Eh_l) P_l)
T4_l ~= Cum_4((h_l-Eh_l) P_l).
```

The basis `P_l` is the leading left-singular subspace of the realised
mean-gated Jacobian from layer `l` to the final output.  The core is
gauge-equivariant: an orthogonal basis rotation rotates every core mode and
cancels in each directed query.

Across a realised weight and ReLU, inherited connected state is transported
by

```text
B_l = P_(l-1)^T W_l D_l P_l
Tn_l(inherited) = Tn_(l-1) x_1 B_l ... x_n B_l.
```

Each layer then adds the complete projected nonlinear birth measured by a
fixed, target-free 512-node Gaussian cubature:

```text
birth_n =
    Cum_n((ReLU(Z)-sample_mean) P_l)
  - Cum_n(((Z-EZ) D_l) P_l).
```

The common-random-number linear subtraction makes identity-activation birth
zero and removes finite-cubature Gaussian cumulant.  Unlike the killed
leading-tree closure, this state includes loops and three-/four-distinct
interactions and carries their signs through subsequent realised weights.
Unlike endpoint PTCC, it is born and transported at every upstream layer
and never interprets the selected production cloud as a probability law.

The final mean uses coefficient-one K3/K4 Edgeworth response plus the
linearly propagated prior mean innovation.  It has no trained or
target-fitted value.

## Structural diagnostics

The coherence gates passed:

```text
whitened-node maximum mean error             8.54e-17
whitened-node covariance maximum error       6.65e-12
identity-birth maximum absolute                  0
all cores and predictions finite                yes
projected current-meter graph                  18B
projected ceiling                              20B
```

The statistic moved materially.  Final candidate-minus-Price RMS ranged
from `0.00205` to `0.01189`; the final directed K3/K4 query was nonzero on
every row.  Parameter-free physical bounds activated `125..634` times over
the `8,192` layer-neuron outputs per row.  This is not a no-op or a numerical
blowup: despite those substantial shifts, every row improved.  It is an
accuracy-ceiling failure.

## Cost projection

The `18B` value is a conservative current-meter algebraic projection, not a
FlopScope receipt.  Its dominant terms are:

```text
full-covariance Price anchor                         ~2.2B
backward response products and response eigenspaces ~1.6B
31 covariance roots plus 512-node maps              ~3.6B
rank-16 projected third/fourth moment cores          ~4.4B
directed queries and core transports                 ~1.4B
elementwise, movement, and safety allowance          ~4.8B
```

Ordinary arrays are far below the 100 MiB result cap.  Accuracy, not
economics, stops the lane.

## Boundary and surviving information

R1 is a stronger negative boundary than the prior mean/covariance-only
closures.  It demonstrates that a target-free, sign-bearing, all-distinct
connected state can be propagated cheaply and can move all four development
rows in the correct aggregate direction.  Rank-16 downstream response plus
a first-order Gaussian-birth recurrence nevertheless recovers only
`43--69%` of the already-poor Price error, when a viable floor-compute
estimator needs roughly `98%`.

Reopening this exact recurrence by adding rank or cubature points is not
justified.  A materially new successor would need a nonperturbative
connected-state law—one that changes the conditional distribution carried
through ReLU, rather than adding another first-order cumulant core to a
Gaussian base.  That is the missing observable; no such candidate is
claimed here.

## Frozen artifacts

```text
preregistration
  52e41b62689d1a084c6897adf19216b5862f38a1ee07a08806c30f2f8b987129

capture source
  ab39758db1fe31f8c7ae5d6c8a866f31082761a1e4802639d1c608399315de6d

target-free prediction archive
  c4a99eef663953b839bce3dd0306d08145e3a27dbd5ef3f360a38ed130f23b52

target-free manifest
  dfcff823a54efb4c7636a18a4d4053dd2143d10fcd1bdf605ceec0e63104e86e

post-seal scorer source
  e38b6bc618e526577b3e5c3e5d1576e394e3cca498f6d1aaf9e7b29076fec267

post-seal score
  059e2cea6e4d0c256e6ceb011fcc594ac5d0be0aa11c716eacf4d7def56013a9
```
