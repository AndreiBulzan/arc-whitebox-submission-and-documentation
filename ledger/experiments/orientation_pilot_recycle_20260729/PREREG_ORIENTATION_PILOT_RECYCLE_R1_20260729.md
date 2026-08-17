# Orientation-pilot recycling R1

Date: 2026-07-29

Evidence target: **component**.  Compute and adjusted-score arithmetic are
**projections**.  This is a cache-only falsifier: no FlopScope session,
physical row, package, upload, submission, or remote action is authorized.

## Question

The existing eight-orientation selector spends `p` full-depth pilot bases in
every orientation, selects two orientations, and then discards the six
unselected pilot outputs from its final estimate.  Can the same computed
support do better if every computed basis is retained with literal equal
weight?

For total support `K`, pilot size `p`, and selected-orientation support `m`,

```text
K = 8p + 2(m-p) = 2m + 6p.
```

The frozen rule is:

1. use the first `p` bases of the existing target-free S109 support in all
   eight orientations;
2. form the coordinatewise trimmed pilot consensus (drop the minimum and
   maximum orientation value);
3. select the orientation pair whose equal pilot mean is closest to that
   consensus;
4. continue only those two orientations through bases `p..m-1`; and
5. return the literal equal mean of all `K` computed basis endpoints.

No pilot is counted twice, no fitted coefficient is used, and no target,
family identity, row identity, or seed participates in routing.

## Frozen grid

Evaluate every valid `(K,p,m)` from:

```text
K in {84, 100, 116, 132, 148}
p in {3, 5, 9, 13, 17}
m = (K - 6p) / 2
```

subject to integer `p <= m <= 109`.  Report all points.  This touched grid
cannot directly promote a candidate.

## Kill/continue rule

Continue this mechanism only if at least one pre-frozen point, separately on
Full100 and Generated64:

- has signed-final-preactivation pooled MSE ratio `<= 0.80` versus current
  K146;
- improves at least `55%` of rows;
- has row-ratio p95 `<= 1.50`; and
- is finite.

The first-order half-gate final-readout diagnostic must also improve both
families.  A pass licenses a disjoint preregistered confirmation and an exact
full-readout capture; it is not a runnable estimator or a score claim.

