# K129 R73 — pair-adjacent reshape-first cold ingress

Date: 2026-08-03

Initial evidence label: **component**.

## Prior-art and new observable

The exact reshape-first construction is established in the K146 R11 builder
and the K162 reshape-first capsule successor.  It is not present in the exact
R71 archive: its vendored
`candidate_r40_direct_cellwave_pairadjacent_v091_20260727.py` remains the
original `9aea0318...` source and creates one getitem-plus-transpose pair per
ingress cell.  The post-R69 census attributes 418 initialized-row getitems to
this owner.  R73 is therefore a narrow port of proven exact prior art, not a
new algebra experiment.

## Frozen change

Only while the deferred production owner is constructed, replace
`PairAdjacentLayoutMixin._build_pairadjacent_ingress_sources` with the audited
reshape-before-transpose hierarchy:

1. reshape each natural bank to `(6,2,row_leaf,9,2,column_leaf)`;
2. transpose to `(9,6,2,2,row_leaf,column_leaf)`;
3. expose the exact 9-by-6 cell grid with hierarchical `unstack`;
4. retain every hierarchy parent on the numerical owner.

Logical cell order, prediction operations, destinations, floating-point
association, and the R71 final output must remain unchanged.  This setup-only
rewrite may add the already-known metadata/view FLOP count.

## One-pass gates

1. Exact archive through the official bare-setup initialized/steady gate.
2. Both predictions equal the R71 hash
   `de892ae29a61e837d247ae408e72bc339e0c12cd367cac7a12d62f8846919014`.
3. Steady count and requests remain `138444109252` and `33502`.
4. Initialized requests improve materially from R71's `40535`; require at
   least 300 fewer requests.
5. If all four pass, take one five-lane initialized/steady screen.  Promote
   only if the calibrated remote initialized and steady tails remain below
   60 seconds.

No upload or submission is authorized.
