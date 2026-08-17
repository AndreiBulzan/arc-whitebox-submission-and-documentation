# Output-fold Stein basis calibration S1/R1 verdict

**Date:** 2026-08-10  
**Evidence:** target-free all-100 candidate seal followed by broad statistical
official-Mini100 scoring  
**Decision:** kill this exact output-fold moment-consistency spelling

## Result

The target-free builder formed 308 cross-output-fold basis-weight cells from
the exact already-paid 129 basis endpoints and final supplied weight columns.
It used degree-zero, first-Hermite, and diagonal second-Hermite consistency
blocks, with nonnegative weights, ESS floors of 64/96, and maximum weights of
2/129 or 4/129. No target was opened before the complete candidate bank was
sealed.

The public-selected cell was `m1_m2`, ridge `0.001`, ESS floor `64`, cap
`2/129`. Relative to exact R92 it produced:

- public raw ratio: `4.11613549` (`1/50` rows improved);
- untouched-holdout raw ratio: `3.34514741` (`6/50` rows improved);
- pooled raw ratio: `3.68518437` (`7/100` rows improved).

Even the hindsight pooled-best cell had raw ratio `3.68507742`. A subsequent
diagnostic allowed an arbitrary nonnegative scalar shrink of every sealed
direction. The public optimum was exactly zero for every cell; no positive
step survives. The failure is therefore directional, not merely excessive
step size.

## Interpretation

Independent final-output siblings do expose exact population identities, but
making their low-order moments agree across folds does not identify which
Kerdock basis contrasts reduce spherical integration error. The resulting
basis-weight directions point away from the true correction on official
Mini100. This closes the exact moment-consistency construction, not all
possible uses of final-output siblings.

The final source audit also confirmed that the live post-layer-23 tail already
returns zero prefixes for layers 1--31 and computes only the scored final
readout. There is no unneeded intermediate-layer readout to delete.

No physical row, package, upload, submission, or remote action was taken.

## Evidence

- `runtime/artifacts/outputfold_stein_basis_s1_r1_targetfree_20260810.npz`
- `runtime/artifacts/outputfold_stein_basis_s1_r1_targetfree_20260810.json`
- `runtime/artifacts/outputfold_stein_basis_s1_r1_postseal_20260810.json`

