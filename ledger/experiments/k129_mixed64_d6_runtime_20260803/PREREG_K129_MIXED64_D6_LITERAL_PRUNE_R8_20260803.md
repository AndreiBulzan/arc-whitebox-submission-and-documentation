# K129 mixed64/D6 literal-sidecar prune R8

Evidence is **component** until the target-free prediction is sealed and
scored.  Compute statements before a physical whole are **projections**.

## Prior-art preflight

Queries covered `matched48`, `literal48`, `sparse D6`, `support 48`,
`direction thinning`, `q0 anchor`, `self center`, and the mixed64 runtime.
The old matched48 candidate selected 48 atoms from the idealized four-frame
proxy and then rebuilt a 48-base literal arm.  It transferred poorly.  No
experiment has pruned the already captured, independently repaired literal64
features themselves while retaining the promoted q0-core64 anchor.  Direction
thinning at 192/160/128 and both q0-anchor removals remain closed.

Outcome: **new observable/support rule**, not a rerun of matched48.

## Frozen target-free selection

- Input features: the broad literal64/D6 proxies sealed in
  `literal_sparse_d6_r4_broad_targetfree_20260802.npz`.
- Anchor: the exact q0-core64 affine anchor sealed in
  `q0_anchor_coreset_r4_targetfree_20260803.npz`.
- Teacher: the target-free complete four-frame endpoint.
- Candidate sizes: exactly `48, 52, 56, 60, 64`.
- Support path: family-balanced OMP over the 64 literal features.
- Coefficient ridge: exactly `1.0` relative trace ridge.
- Validation: four folds by stored row ordinal, balanced equally across
  Full and Generated families.
- Physical price used only for selection:

  `E(s) = 137.926260702B + 2.000000000B + 16.904360918B * s / 64`.

  The 2B fixed term deliberately keeps the q0 anchor, frame construction,
  and fixed bookkeeping from receiving a fictitious thinning discount.
- Selection metric: worst-family OOF teacher-reconstruction MSE multiplied
  by `E(s) / E(64)`, then smaller support as tie-break.

No challenge target, row loss, or post-seal score may select the support.
After selection, rebuild that support as a literal arm because H1/H2/L4
repairs depend on support cardinality.  Refit the coefficient target-free,
seal, and only then open the two broad target banks.

## Promotion gate

Promote to a physical R35 only if the post-seal central adjusted projection,
using a conservative physical price, is below R34 on both families and at
most `1.11e-7` on Generated128.  Otherwise kill the complete lane without
size or ridge retuning.

No package, upload, or submission is authorized.
