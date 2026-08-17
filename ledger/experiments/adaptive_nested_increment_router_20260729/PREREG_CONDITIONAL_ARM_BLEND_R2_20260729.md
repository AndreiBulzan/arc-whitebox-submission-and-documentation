# Conditional K146 arm blend R2

Date: 2026-07-29

R1 support routing failed its reciprocal-family gate. Before closing the
adaptive lane, R2 tests the only same-cloud alternative: predict a
network-specific scalar O1 arm weight from coordinate-symmetric summaries
of the already computed K146 O0 and O1 outputs.

This protocol was frozen after observing only the target-informed rowwise
scalar capacity. That capacity is an upper bound: Full100 retained 86.99%
and Generated64 retained 80.85% of literal raw MSE. No fitted result was
observed before freezing this protocol.

## Inputs and evidence

Use the frozen K146 predictions, the support-invariant O0 endpoint from the
frozen endpoint grid, and already opened Full100/Generated64 targets. O1 is
algebraically reconstructed from the literal K146 blend. No estimator,
FlopScope, physical row, package, network, upload, or submission may run.
Accuracy is `broad statistical`; compute and remote transfer are
`projection`.

## Rule

Inference features are coordinate-symmetric summaries of O0, O1, their
difference, and the literal blend. They include moments, quantiles, norm
ratios, centered correlations, and sign agreement. Row identity, seed,
family, weights, and targets are forbidden at inference.

Fit the exact row-optimal scalar coefficient as the supervised response.
Select, using deterministic five-fold training-family out-of-fold score:

- standardized ridge with alpha in
  `{0.01,0.1,1,10,100,1000}`;
- extra trees with depth in `{2,3,None}` and minimum leaf in `{3,5,10}`;
- output clipping in `none`, `[-0.25,0.5]`, `[0,0.5]`, or `[0,1]`.

Refit the selected training-only protocol and apply it unchanged to the
opposite family. Evaluate both reciprocal directions.

Charge `0.25 B` effective work per row. Translate each held-family raw-MSE
ratio to a remote projection by multiplying submission 320262's score by
that ratio and by `(171.000341928 + 0.25) / 171.000341928`.

## Gate

Pass only if both reciprocal remote-calibrated projections are
`<=1.20e-7`, and both held families improve the literal blend. Otherwise
close adaptive routing/blending around the frozen K146 endpoint.

