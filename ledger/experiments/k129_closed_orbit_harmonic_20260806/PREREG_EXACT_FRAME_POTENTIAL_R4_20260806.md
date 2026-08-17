# Exact fourth-order frame-potential interlace R4

Date: 2026-08-06

Evidence sought: target-free ordinary-CUDA **component** audit on Full2 and
Generated2, followed by a post-seal score only if the exact tensor gate
passes.  No FlopScope session, production estimator edit, Mini100 access,
package, upload, submission, or remote action is authorized.

## Reopening boundary

R3 is killed.  Its 64 random fourth-moment probes overfit their own sketch
and the resulting estimator reversed on Generated.  R4 does not increase the
probe count or retune that sketch.  It removes the sketch entirely.

For complete orthonormal bases `B` and `C`, the exact fourth-tensor inner
product is

```text
<T_B,T_C> = mean_{i,j} (B_i dot C_j)^4.
```

Consequently the squared error of the complete mixed rule's fourth-moment
tensor is, up to a fixed normalization, its fourth frame potential minus the
spherical Welch/Delsarte lower bound.  Every term is computable exactly from
the basis overlaps; no target, trajectory, random direction, or learned
coefficient enters.

R4 forms the four closed-orbit phases `0, pi/4, pi/2, 3pi/4`, computes all
`C(129,2)*16` cross-basis fourth energies, and minimizes their exact sum by
deterministic balanced phase swaps from eight frozen starts.  Counts remain
`33/32/32/32`.

## Fixed scout and gates

```text
Full       472, 475
Generated   80,  83
```

The first gate is target-free.  Continue to literal propagation only if the
optimized fourth-tensor excess is at most `0.25` of fixed cyclic interlace on
every row.  The canonical all-phase-zero K129 rule must reproduce the exact
lower bound to relative error `<=1e-5` as an implementation check.

If and only if that gate passes, freeze q0, cyclic and exact-potential
predictions before opening targets.  Promote to a disjoint 8+8 expansion only
if the optimized rule then has:

1. pooled raw-MSE ratio to q0 `<=0.85` in both families;
2. at least `3/4` rows improved; and
3. maximum row-loss ratio `<=1.5`.

Failure closes this exact frame-potential/balanced-swap spelling.  It may not
be rescued by changing starts, phase counts, rows, or thresholds.
