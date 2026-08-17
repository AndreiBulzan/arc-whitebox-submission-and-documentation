# Preregistration: Haar16 direct layer-31 mean calibration R10

R9B/R9C killed the Gaussian random-feature route from a Haar16 layer-31 mean
query to final basis weights.  They did not test the direct sufficient
statistic: the exact 256-vector mean of each already-propagated K129 basis.

R10 is a post-hoc mechanism screen on the already-open R9B 32-row bank.  A
new target-free capture stores only the 129 basis-level layer-31 mean vectors
and verifies their uniform mean against R9B's sealed K129 mean.  R9B already
contains the two Haar16 query means and 129 actual final-layer basis endpoints.

For shrink values `{0.1,0.25,0.5,0.75,1.0}` and relative ridges
`{1e-6,3e-6,1e-5,3e-5,1e-4,3e-4,1e-3,3e-3,1e-2,3e-2,1e-1,3e-1,1,3,10}`,
solve one shared-output 129-weight constrained ridge per network:

`sum(w)=1`, minimizing the layer-31 mean mismatch plus ridge to uniform.

Evaluate two deterministic weight modes:

1. raw equality-constrained ridge;
2. a ray-bounded version moved from uniform only as far as needed to keep
   `0 <= w_b <= 4/129`.

Choose one global shrink/ridge cell per mode target-free by opposite-replica
mean risk: fit to Haar replica 1 and predict replica 2, then swap.  Seal every
cell's final prediction and the selected cells before a separate scorer reads
the already-open targets.

This bank can establish capacity or kill the mechanism but cannot validate a
post-hoc survivor.  Any useful tuple must be frozen and confirmed on another
disjoint bank.  No physical, Mini100, remote, upload, or submission action is
authorized.
