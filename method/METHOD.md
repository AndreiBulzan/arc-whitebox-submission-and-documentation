# What the current estimator does

## Current R90 live-graph correction

The current validated local successor is R90, whose sealed numerical owner is
**K129 O0-only**: 129 bases, 66,048 propagated rows, no O1 arm and no outer
blend. It retains the signed-final-preactivation endpoint correction with
`lambda=0.0075`. The K238/K162 discussion below documents the older estimator
family and its statistical ancestry; it is not the live R90 execution graph.
The archive-derived receipt is
`runtime/artifacts/r90_live_graph_manifest_e0_r1_20260809.json`.

## Core numerical idea

For the current K258/K238 family and frozen oracle banks, pure
mean/covariance propagation treats each layer's preactivations as too
Gaussian. The final-readout residual is overwhelmingly localized in signed
final preactivation mean. Directed non-Gaussian pair-covariance births at
ReLU boundaries are one demonstrated upstream contributor, explaining about
55% under the fixed receiver; they are not proven to explain the whole
residual.

Our accurate family uses deterministic structured sampling as a mechanistic
quadrature:

1. Generate balanced Kerdock/Hadamard-like directions rather than IID inputs.
2. Propagate their activation cloud through all ReLU layers.
3. Preserve paired/orientation structure so odd and even errors cancel.
4. Repair early H1/H2 and layer-4 moments independently for two orientation
   arms.
5. Carry a compressed width-216 middle where broad tests show the accuracy
   loss is small.
6. Reconstruct late signed products and covariance-sensitive readout
   features.
7. Form separate arm predictions and blend them by their literal support
   weights.

The K238 statistic uses 129 Kerdock bases in one orientation and 109 in the
other, yielding 121,856 propagated signed rows in total. Keeping their
repairs and readouts independent matters; collapsing them into one
exchangeable cloud loses signal.

## Why it works

- Balanced directions reduce quadrature variance much more efficiently than
  IID sampling at the same support size.
- Deterministic propagation preserves weight-specific geometry that a
  Gaussian closure discards.
- H1/H2 and layer-4 repairs restore moment information damaged by compressed
  physical propagation.
- Late signed/covariance features target the demonstrated oracle gap rather
  than spending samples uniformly.
- The two-arm blend reduces orientation-specific errors without fitting
  targets per MLP.

The estimator is best viewed as a structured quadrature plus moment-repair
method, not as ordinary Monte Carlo and not as a pure cumulant closure.

## Oracle localization

An exact-target oracle on the current geometry isolates where the remaining
accuracy is available:

```text
current estimator raw                         about 1.4328e-7
replace signed final preactivation mean             4.499e-9
also replace radial second moment                    1.679e-9
replace final post-ReLU mean                          4.912e-10
```

This does not make those states cheaply observable. It does show that about
`96.9%` of the present raw error is linked to the signed final preactivation
mean, with further signal in covariance/radial structure created at ReLU
boundaries. Marginal K3/K4 propagation did not recover this gap reliably.
The useful research question is therefore how to estimate those directed,
weight-specific non-Gaussian innovations without paying for a much larger
activation cloud.

## Accuracy and compute are separate claims

The K238 broad prediction is accurate enough to be interesting. The current
physical graph is still too expensive because FlopScope 0.9.1 honestly prices
movement, dtype width, large contractions, and residual Python/lifecycle
work. Algebraic equivalence does not imply equal score or wall.

Useful lawful implementation devices already demonstrated:

- `fnp.vecmat(..., out=...)` for the best verified contraction surface;
- persistent buffers and setup-bound basic slices;
- result-cap-aware segmentation rather than giant intermediates;
- pair-packed and rank-segmented cell banks;
- setup-bound straight-line operand dispatch;
- separate exact ownership maps for every reused boundary.

Every rewrite must preserve operation association closely enough to match
the frozen prediction. Floating-point reassociation can change the estimator
even when the symbolic formula is identical.
