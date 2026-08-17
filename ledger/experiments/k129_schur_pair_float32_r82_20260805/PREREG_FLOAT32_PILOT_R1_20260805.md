# R82 float32 constructor pilot R1

Date: 2026-08-05

Prior-work search found no float32 polar/eigenframe constructor experiment.
R81's measured constructor is 2.394123916B and its decompositions are billed
at the float64 2x dtype rate.  This candidate evaluates the identical
gamma=0.50 construction entirely in float32, then propagates the unchanged
K129 trajectory.

The frame is not assumed prediction-preserving: a preliminary target-free
frame comparison found large rotations within nearly degenerate eigenspaces.
It is therefore a new statistical candidate.  The fixed pilot is the first 16
sealed R4 rows in Full and Generated and Mini rows 0..15.  Promotion requires
raw ratio versus q0 <=0.95 on every family, pooled <=0.94, and at least 8/16
rows improved in each family.  Only then may it run the full populations.

