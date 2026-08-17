# Sparse cubic H1 GREG R1 preregistration — 2026-08-01

Evidence class: component pilot only.  This is not a physical benchmark row,
broad statistical evidence, or a remote result.

## Purpose

Reconstruct the independently reported sparse-cubic/SVD/GREG signal on the
sealed K129 pilot16 without consulting final-layer targets during feature
construction.  The useful threshold for the current checkpoint is a pooled
raw-MSE ratio at or below `0.93`: combined with R27 and the existing low-wall
exact contraction saving, that is large enough to project below `1.10e-7`.

## Target-free construction

For each MLP:

1. Rebuild the literal 129 complete H1 frames used by the K129 estimator,
   including its compact affine first-layer repair.
2. Estimate mean downstream ReLU gates from the fixed basis ordinals
   `(0, 16, 32, 48, 64, 80, 96, 112)` only.  Pull the final linearization back
   to H1 and take its leading left singular directions.
3. For each leading direction, select the three distinct H1 coordinates with
   greatest absolute loading.  Sort coordinate triples and discard repeats.
   Continue through the singular directions, and deterministically fill from
   adjacent top-coordinate combinations if required, until 16 triples exist.
4. The control for triple `(i,j,k)` is the complete-frame average of
   `h1_i*h1_j*h1_k`.  Its population value is computed independently from the
   first-layer weights and affine repair.  The raw three-ReLU spherical moment
   is evaluated by deterministic tensor Gauss-Hermite quadrature at orders 40
   and 56; the order-56 value is used and the disagreement is recorded.
5. Calibrate the 129 frame weights with the same PCR/GREG rule already used by
   the capsule, then apply those weights to the sealed per-frame O0 endpoints.
   No target, teacher, final error, or output residual is used in steps 1--5.

## Fixed screen

The post-seal scorer reports ranks `(2,4,8,12,16)` and damping
`(0.25,0.5,1.0)` for mechanism diagnosis.  The primary candidate is
`full_p8_eta0.5`; a secondary candidate may be selected from the finite ladder
only for a subsequent, disjoint broad confirmation, never promoted directly
from this pilot.

The primary passes this pilot iff all hold:

- pooled raw-MSE ratio `<= 0.93`;
- at least 10 of 16 MLPs improve;
- worst per-MLP ratio `<= 1.75`;
- all exact-control order-40/order-56 disagreements are `<= 2e-7` in absolute
  value.

If the primary misses, the exact spelling is killed.  A secondary ladder hit
may justify one frozen broad confirmation only when it meets the same gates.

No remote action or upload is authorized by this preregistration.
