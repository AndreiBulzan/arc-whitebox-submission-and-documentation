# Fourth-moment-restored phase interlace R3

Date: 2026-08-06

Evidence sought: target-free ordinary-CUDA **component** capture followed by
a separate post-seal Full4/Generated4 score.  No FlopScope session,
production estimator edit, Mini100 access, package, upload, submission, or
remote action is authorized.

## Reopening boundary

The unchanged R2 closed-orbit rule failed its deliberately strong aggregate
promotion gate because its 16/8-angle Generated ratios were `0.613/0.648`
versus `0.55/0.60`.  It is not promoted.  Its separately frozen four-unique-
phase teacher nevertheless had raw ratios `0.202` Full and `0.648`
Generated for the eight-angle mean; the four-angle mean improved `27/32`
rows.  That is capacity evidence only.

`k129_whole_basis_interleave_20260805` is the controlling same-budget
negative: fixed alternating/block/cyclic assignments of complete bases
destroyed the design and reversed on both broad families.  R3 changes the
controlling assumption.  It treats the phase assignment as a constrained
fourth-moment discrepancy problem and selects it without targets.

Prior-art outcome: **new execution surface invalidating the fixed-Latin
assumption**.  This is not a continuation of the failed R2 teacher gate and
not another arbitrary basis allocation.

## Exact structural fact used

Every antipodally completed orthonormal basis has exact mean, zero odd
moments, and exact covariance.  Therefore assigning different orthonormal
bases to different rotations preserves all spherical identities through
degree three automatically.  The first identity mixing can break is the
fourth moment.  The original maximal-MUB K129 design makes that fourth moment
exact, and antipodality then supplies degree five.

R3 uses the four distinct phase sets from the successful eight-angle orbit:
angles `0, pi/4, pi/2, 3pi/4`.  Exactly `33/32/32/32` complete bases are
assigned to them.  For 64 fixed Gaussian unit probes, compute every
basis/phase fourth-moment contribution and minimize the normalized aggregate
discrepancy by deterministic balanced pair swaps from two fixed starts.  A
second 64-probe set is validation-only.  The chosen assignment is then used
unchanged in both structured-block calls of the real K129 front end.

## Fixed scout

```text
Full       448, 451, 454, 457
Generated   64,  67,  70,  73
```

Freeze q0, fixed cyclic phase interlace, and moment-optimized interlace
predictions before opening targets.  Promote to a disjoint 16+16 expansion
only if:

1. optimized validation-probe fourth-moment loss is at most `0.25` of the
   fixed cyclic loss on every row;
2. optimized raw-MSE ratio to q0 is at most `0.85` in each family;
3. at least `6/8` rows improve; and
4. no optimized row loss exceeds `1.5x` q0.

At remote-R87 price, unchanged-compute sub-`1e-7` needs raw ratio about
`0.875`.  The `0.85` gate leaves a small allowance for phase selection and
layout overhead.  Failure kills this exact random-probe/pair-swap spelling;
it cannot be rescued by changing probe seeds, counts, rows, or swap starts.

