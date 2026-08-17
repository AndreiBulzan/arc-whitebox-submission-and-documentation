# Nested-increment adaptive support router R1

Date: 2026-07-29

This is an offline, kill-fast development-bank test. Accuracy evidence is
`broad statistical`; all compute values are `projection`. It may read only
the already frozen target-free K146 and six-support endpoint archives and
their already released score arrays. It must not run an estimator,
FlopScope, a physical row, packaging, network access, upload, or submission.

## Distinct question

Prior routers used weights, the common O0 endpoint, or one completed arm's
internal disagreement. R1 asks whether the observable convergence increment

```text
P33 - P17
```

predicts which larger support is worth buying. The two predictions can be
formed from the union of the frozen S17 and S33 O1 supports. Those supports
overlap in 16 of 17 S17 bases, so the union has 34 O1 bases.

The router may use only coordinate-symmetric summaries of `P17`, `P33`,
their increment, reconstructed O1 endpoints at S17/S33, and O0/O1
disagreements. Row identity, seed, family identity, weights, targets, and
target-derived errors are forbidden inference features.

## Honest optimistic price

Every row first pays for the 34-base union. A row routed to support `m` pays
for the union `S17 ∪ S33 ∪ Sm`. Effective work is linearly projected from
the remote K146 and failed-remote K162 diagnostics:

```text
C17 = 171.000341928 B
C33 = 185.131340877 B
per additional O1 basis = (C33 - C17) / 16
```

An additional `0.25 B` is charged for summaries and the decision. This is
optimistic: it omits any duplicate support-specific repair/readout work.
Consequently failure under this price is decisive; passage is only a lead
requiring a static graph cost audit.

Each row is scored literally as
`row_mse * max(0.1, projected_effective / 272 B)`.

## Frozen models and transfer test

Candidate supports are `m = 33,49,65,73,85,97`.

The following model classes are fixed before the formal run:

1. multi-output ridge regression, with alpha selected from
   `{0.01,0.1,1,10,100,1000}` by deterministic five-fold
   training-family out-of-fold adjusted score;
2. extra-trees multi-output regression, selected by training-family
   out-of-fold score from depth `{2,3,None}` and minimum leaf `{3,5,10}`;
3. a single-feature depth-one support rule, selected entirely on the
   training family.

For each direction, select the class and hyperparameters using training
family out-of-fold score only, refit on the complete training family, and
evaluate once on the untouched held family. Run both Full100→Generated64
and Generated64→Full100.

Also evaluate the scalar Richardson/extrapolation coefficient for
`P33 + alpha*(P33-P17)`, with alpha fitted on the training family from the
exact quadratic identity and applied unchanged to the held family.

## Hard gate

Promote only if one fixed model-selection protocol achieves all of:

- held mean adjusted score `<= 1.20e-7` in both reciprocal directions;
- more than one selected support in both held families;
- no worse than the held family's best fixed support;
- the same feature and model protocol in both directions.

Otherwise close this adaptive-compute lane. Oracle support selection is
reported only as capacity and can never pass the gate.

