# K146 multilevel width correction R1

Date: 2026-07-28

Evidence sought: **component** target-free CUDA capture, followed by one
sealed two-family **component** scorer. Count and effective-compute values are
**projection** only. No FlopScope, physical row, package, network, remote,
upload, or submission action is authorized.

## Hypothesis

The previously measured width cliff may be mostly a correlated compression
error rather than irreducible loss. Propagate all 146 Kerdock bases through a
cheap mean-folded path, and estimate the missing high-width contribution from
a balanced subset of bases:

```text
Q_ML = Q_low(all bases)
     + 129/146 * mean_S0(Q_high,b - Q_low,b)
     +  17/146 * mean_S1(Q_high,b - Q_low,b).
```

`Q_high,b` and `Q_low,b` are the per-basis lambda-zero readout endpoints.
Positive and negative halves of every selected basis are always kept
together. The high endpoint is the incumbent energy-ranked `s12/w216` path;
the low endpoint is one of:

```text
obsvar_s12_w192
obsvar_s07_w184
```

This is a coupled multilevel correction. It fits no coefficient and reads no
target during capture.

## Frozen rows

These are the already used sensitivity-screen rows, chosen so the new
mechanism consumes no untouched lower-K confirmation target:

```text
Full       211, 223, 227, 239
Generated   13,  20,  24,  27
```

The scorer may open only these eight targets, and only after the complete
prediction archive and manifest are sealed.

## Frozen balanced supports

For an arm with `K` bases and requested subset size `m`, select the center of
each of `m` equal position strata:

```text
position_j = floor((j + 1/2) * K / m),  j=0,...,m-1.
```

This is deterministic, target-free, and independent of weights. O0 positions
are also its basis identifiers. O1 identifiers use the sealed m17 support
`[6,9,11,37,41,42,48,52,55,58,64,65,79,108,111,112,128]`.

Primary subsets:

```text
nominal 1/8:
  O0 positions/ids = [4,12,20,28,36,44,52,60,68,76,84,92,100,108,116,124]
  O1 positions     = [4,12]
  O1 ids           = [41,79]
  bases / rows     = 18 / 9,216

nominal 1/4:
  O0 positions/ids = [2,6,10,14,18,22,26,30,34,38,42,46,50,54,58,62,
                      66,70,74,78,82,86,90,94,98,102,106,110,114,118,
                      122,126]
  O1 positions     = [2,6,10,14]
  O1 ids           = [11,48,64,111]
  bases / rows     = 36 / 18,432

nominal 1/2:
  O0 positions/ids = [1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31,
                      33,35,37,39,41,43,45,47,49,51,53,55,57,59,61,63,
                      65,67,69,71,73,75,77,79,81,83,85,87,89,91,93,95,
                      97,99,101,103,105,107,109,111,113,115,117,119,
                      121,123,125,127]
  O1 positions     = [1,3,5,7,9,11,13,15]
  O1 ids           = [9,37,42,52,58,65,108,112]
  bases / rows     = 72 / 36,864
```

A nominal `1/16` diagnostic is frozen before target access because it is free
once per-basis endpoints exist:

```text
O0 positions/ids = [8,24,40,56,72,88,104,120]
O1 position/id    = [8] / [55]
bases / rows      = 9 / 4,608
```

The subsets are intentionally centered and need not be nested. No support is
changed after target access.

## Capture semantics

For each row, CUDA replays exactly three all-basis trajectories:

```text
incumbent energy_s12_w216
low obsvar_s12_w192
low obsvar_s07_w184
```

The capture freezes the `(bases,256)` per-basis readout endpoints for both
arms and all three trajectories. Candidate predictions are composed from
those endpoints using the equation above. This is an optimistic
algorithmic kill-test: the high endpoints use the complete incumbent
all-basis dynamic late selections. A deployable subset-only implementation
must separately prove that its coupled late-selection spelling preserves the
observed correction.

The incumbent endpoint mean must associate with the existing GPU control on
Full0 at relative RMSE `<=2e-6` and maximum absolute error `<=3.2e-5`.

## Cost projection

The all-basis low-path counted projections are inherited from the sealed
sensitivity audit:

```text
s12/w192  130,737,337,812
s07/w184  111,760,605,492
```

The extra high branch is priced as an ordinary public dense-matmul branch on
the selected signed rows. It includes:

- incumbent widths from the fork through W21;
- W22--W23 at width 200;
- seven `192 -> 200` late products;
- the `176 -> 256` final product;
- a conservative basic gather and segmented readout allowance.

Holding the remote R17 residual contribution fixed at
`171,000,341,928 - 144,015,328,956 = 26,985,012,972` effective units gives
the preregistered economics:

| low path | subset | projected count B | residual-held C B | raw ratio ceiling for adjusted `1.2e-7` |
|---|---:|---:|---:|---:|
| s12/w192 | 1/8 | 146.615 | 173.600 | 0.8994 |
| s12/w192 | 1/4 | 162.492 | 189.477 | 0.8240 |
| s12/w192 | 1/2 | 194.246 | 221.231 | 0.7058 |
| s07/w184 | 1/8 | 132.599 | 159.584 | 0.9784 |
| s07/w184 | 1/4 | 153.436 | 180.421 | 0.8654 |
| s07/w184 | 1/2 | 195.111 | 222.096 | 0.7030 |

The ratio ceilings use remote raw `2.0904670478e-7`. They are projections,
not receipts. The same table and the diagnostic 1/16 values must be
recomputed by source and sealed in the capture manifest.

## Go/kill gates

For each candidate, score literal final-layer MSE against the incumbent
endpoint prediction on both families.

A point is a `1.2e-7` checkpoint lead only if:

1. it improves or ties the incumbent on at least two of four rows in each
   family;
2. no row ratio exceeds `2.0`;
3. each family-pooled raw ratio is at or below that point's preregistered
   residual-held ratio ceiling; and
4. its projected count is below the challenge's 272 B counted limit.

The primary decision order is:

```text
s07/w184 1/8
s12/w192 1/8
s07/w184 1/4
s12/w192 1/4
s07/w184 1/2
s12/w192 1/2
```

The 1/16 rows are diagnostic and may motivate a separately confirmed
successor, but cannot be promoted from this eight-row score alone.

Failure kills this exact centered-strata output-level correction. Passing is
only a component lead: it still requires a subset-feasible late-selection
capture, broader Full/Generated scoring, an exact FlopScope graph, and the
ordinary validation ladder.
