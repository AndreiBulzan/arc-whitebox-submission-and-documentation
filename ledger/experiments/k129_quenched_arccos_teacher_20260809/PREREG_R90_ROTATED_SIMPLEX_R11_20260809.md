# Preregistration: R90 plus rotated regular-simplex queries R11

R10F proved that independent rotated full-network views contain a general
correction signal, but a 512-row antipodal orthogonal basis costs more than
the transferred raw gain.  R11 replaces each query basis by a 257-point
regular simplex in `R^256`.

Every rotated simplex has unit-norm rows, row sum zero, and

`Q^T Q / 257 = I / 256`.

It therefore integrates constants, all linear spherical functions, and all
quadratic spherical moments exactly, using almost half the rows of a
512-point cross-polytope.  Haar rotation makes every vertex marginally
uniform on the sphere.

This first experiment is a development count curve on the already-open R9B
bank (`Full676..691`, `Generated72..87`).  Reuse the sealed exact R90/q0
predictions from R10E.  For two deterministic independent rotation replicas,
capture cumulative exact final-output means at `1,2,4,8,16,32` simplexes.
The capture may read weights only.

After sealing the query means, estimate reliability without benchmark
targets.  Let `d_r(k)=simplex_r(k)-q0`.  Estimate the common squared signal by
the cross-replica inner product at the least noisy count 32:

`S = max(<d_0(32), d_1(32)>, 0)`.

For a single replica at count `k`, use

`beta_k = clip(2 S / (||d_0(k)||^2 + ||d_1(k)||^2), 0, 2)`.

For the average of both replicas, use

`beta_bar_k = clip(S / ||(d_0(k)+d_1(k))/2||^2, 0, 2)`.

Seal q0 plus these corrections for replica 0, replica 1, and their average at
every count.  Only then read the already-open development targets.

Price one simplex by the transparent row-ratio projection
`1.08B * 257/512 = 0.542109375B`.  Freeze a count/replica spelling for a new
disjoint bank only if both families improve and projected adjusted score is
below the R90 reference `1.1307520123575174e-7`.

No physical row, Mini100 row, package, upload, submission, or remote action is
authorized.
