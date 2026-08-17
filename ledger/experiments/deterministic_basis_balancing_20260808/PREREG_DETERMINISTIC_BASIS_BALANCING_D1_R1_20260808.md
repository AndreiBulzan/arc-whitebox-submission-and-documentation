# Deterministic complete-basis balancing D1/R1 preregistration

Date: 2026-08-08

Evidence scope: target-free output-oracle **component** selection from the
sealed complete-basis packet atoms, followed by post-seal development
scoring.  Costs are projections.  This is not a production selector,
physical receipt, Mini100 gate, package, upload, or remote result.

## Question

Independent basis sampling failed because 129 individually large correction
vectors must cancel.  Can a deliberately selected equal-weight subset of
complete bases preserve that cancellation at `S=4` or `S=8`?

For basis atoms `Y_a` and total `Delta=sum_a Y_a`, select a subset `A` of
size `S` and approximate

```text
Delta_hat = (129/S) * sum_{a in A} Y_a.
```

Selection minimizes `||Delta_hat-Delta||^2/256` and never reads benchmark
expectation targets.  It is nevertheless an output oracle because the true
packet-response atoms would be unavailable in production.

## Fixed inputs and solver

Use the 32-replicate sealed basis capture on Full rows 640,641 and Generated
rows 88,89.  Test `S=1,2,4,8,16,32`.

For every row independently, and once on the four-row concatenated output,
use:

- 20,000 frozen-seed uniform random subsets;
- the best 32 as swap-descent starts;
- deterministic one-for-one swaps until no strict improvement;
- one forward greedy start.

Run the same fixed solver on each 16-replicate half.  Apply half-trained
supports to the opposite half and to the full atom estimate.  Report support
overlap and residual transfer.  No benchmark targets are opened in the
selection stage.

## Gates and stopping

A universal-support survivor requires at least 35% raw reduction in both
families at `S<=4`, including positive reduction for supports learned from
either replicate half.

A per-row output-oracle survivor at `S<=4` proves capacity only and licenses
research on a lawful basis-feature selector.  It is not a production lead by
itself.  An `S=8` result is recorded as a cost-bridge lead because the current
`2.15B` per-basis projection exceeds 12B.

If even per-row output-oracle selection at `S<=8` cannot retain 35% raw
reduction, deterministic equal-weight complete-basis selection is rejected
at this budget.  That does not reject unequal weights, layer-vector groups,
correlated source estimators, or other collective partitions.

## Integrity

- Refuse overwrite and pin source, preregistration, grouped capture/report,
  and output.
- No Mini100, target access during selection, FlopScope, physical runner,
  package, upload, or submission is authorized.
