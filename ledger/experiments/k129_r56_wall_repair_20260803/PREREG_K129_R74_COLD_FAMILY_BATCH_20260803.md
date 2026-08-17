# K129 R74 — remaining cold family batching

Date: 2026-08-03

Initial evidence label: **component**.

## Prior-art check and target

The exact R69 census leaves two regular initialization-only families after
R71:

- 160 views in the persistent signed-fold constructor; and
- 418 views in the concrete capsule consumer-matmul constructor, of which the
  repeated layer-index families account for 204 calls.

Searches over the capsule for these constructor names plus `unstack`, view
batching, persistent binding, and pair-adjacent batching found the R68/R69
Morton, decoder, and boundary transforms and the older K146 R11 ingress
rewrite, but no rewrite of these two concrete families.  R73 confirmed that
the older ingress rewrite is a no-op on this chassis.  The new observable is
the initialized-versus-steady caller census, which isolates these exact
shapes and repetition counts.

## Frozen change

During deferred setup only, intercept basic indexing for the nine exact
source shapes listed in the source.  Replace each repeated integer-index
family by one shared basic slice followed by hierarchical `unstack` calls.
All returned objects are exact views, and every source/intermediate is kept
alive on the estimator for the worker lifetime.  The R71 rank-axis hook is
composed rather than bypassed.

No numerical operation, value, logical order, destination, or floating-point
association may change.  The hook must be restored before steady execution.

## One-pass gates

1. Official bare-setup initialized/steady archive gate.
2. Both outputs equal R71 byte hash `de892ae29a61e837...`.
3. Counts remain `140690324797` initialized and `138444109252` steady.
4. Steady requests remain `33502`; initialized requests must improve from
   `40535` by at least 180.
5. If all pass, take one five-lane initialized/steady screen and retain only
   if both calibrated remote tails are below 60 seconds.

No upload or submission is authorized.
