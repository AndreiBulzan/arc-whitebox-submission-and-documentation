# K129/R21 near-pass inventory R1

Date: 2026-07-30

Evidence label: **source-static audit** for the inventory and exact integer
formulas quoted from their named audits; **projection** for every newly
combined score below. No estimator was edited, no target or FlopScope
session was opened, no physical benchmark or package was run, and no remote
action occurred.

## Bottom line

The banked base remains R21 (`lambda=0.0075`, including the
`89,949,888`-count CSE/rotation cleanup). The only already-scored composable
addition is the frozen output scale `1.000025`. Its conservative projection
is:

```text
R21                                         1.22715e-7 -- 1.23213e-7
R21 + fixed scale                           1.22485e-7 -- 1.22982e-7
further adjusted gain needed at upper end                 2.425%
```

No other completed K129 verdict supplies that remaining gain. The strongest
immediate *new falsifier* is a depth-selective late-width schedule, described
below. It has crossing-grade count arithmetic but no accuracy evidence yet.

## Ranked routes

### 1. Immediate falsifier: compress early late layers, preserve layer 30

Freeze exactly this joint estimator:

```text
R21 endpoint lambda                         0.0075
late layers 24..29 keep                     160 (from 192)
late layer 30 keep                          192 (unchanged)
final layer 31 keep                         160 (from 176)
fixed final output scale                    1.000025
```

The exact current-graph primary saving from
`VERDICT_K129_KEEP_CONTRACTION_R1_20260729.md` is:

```text
six late layers: 6 * 32 * 18,435,963       3,539,704,896
final keep:       16 * 20,829,898             333,278,368
total                                             3,872,983,264
```

Using the conservative R21 upper anchor `1.2321377e-7` at projected public
effective work `146.990043463B`:

```text
count-only, unchanged raw                    1.199672585e-7
plus frozen scale ratio 0.9981235044          1.197421405e-7
```

The exact joint prediction must be scored; multiplying the old scale ratio
into the new tail is only a sensitivity. The count-only candidate crosses
only if its joint raw ratio versus R21 is at most `1.000272920`. If the old
scale effect transfers, the compression itself may worsen raw MSE by at most
about `0.2153%`. This is a narrow gate, but the schedule deliberately leaves
the most output-sensitive late layer 30 wide.

**Fast falsification:** one preregistered target-free Full8 + Generated8
capture, with the candidate fixed as above and no width grid. Seal outputs,
then score. Kill immediately if either pooled candidate/R21 ratio exceeds
`1.00027`; require materially better than the mathematical boundary before
broadening. This needs no physical meter until accuracy passes.

### 2. Pair-residual teacher distillation: strongest unclosed accuracy idea

The public `rn` pair-residual teacher is the only recent K129 mechanism with
a genuinely new label and a declared `<=0.5B` deployment surface. At
`+0.5B`, it must reach raw ratio `<=0.970616` on both Full and Generated
(at least `2.9384%` raw gain). That would clear the checkpoint.

It is not immediate: the exact teacher plus matching weights is `124.36GB`
and absent locally, and a fresh per-basis K129 state capture is also needed.
Treat this as the best breakthrough research route, not a current candidate.

### 3. Exact bilinear stack: numerical near-pass, operational no-go

The corrected two-part exact-algebra stack saves `3.026043072B`. With the
fixed output scale its conservative projection is
`1.199635e-7--1.204503e-7`; the upper endpoint still needs `0.538192B`.
Both rewrites change float32 association, and the rank-49 W1 replacement
projects a backend wall regression before request cost. It is too much
implementation and wall risk for an optimistic upper score above the
checkpoint.

### 4. Best measured sub-threshold effects

These are real or near-real small effects, but none closes the gap:

| mechanism | strongest defensible effect | sensitivity with scale | decision |
|---|---:|---:|---|
| dynamic keep-120 after L16 | raw-times-compute `0.991025` Full / `0.995508` Synthetic under favorable pricing | worst arithmetic about `1.2243e-7` from the R21 upper anchor | insufficient; accuracy/cost screen already killed |
| across-basis standard-deviation correction | reciprocal-transfer worst raw ratio `0.996334`, row p95 `1.24--1.30` | unsealed product with scale about `1.2253e-7` | insufficient and statistically unstable |
| impossible deletion of every reshape | at most `0.304260037B` amortized | scale-adjusted ceiling around `1.2273e-7` | impossible ceiling, not an implementation target |
| corrected rank-49 alone | `0.5451264B`; theoretical fully fused ceiling plus destinations `1.862690688B` | theoretical best still `1.216521e-7` before requests | hard no-go |
| dead O0 gamma path | `0.000340564B`, 68 numerical calls | negligible | cleanup only |

The old two-basis O1 endpoint proxy is not eligible here: the actual
isolated sparse-O1 trajectories subsequently failed or reversed. The full-17
O1 Edgeworth signal is real (`0.9416`/`0.9283` raw ratios in its named
diagnostic), but its full current-meter trajectory cost overwhelms that gain;
all tested one/two/four-basis, narrow-width, and direction-thinned spellings
were killed.

## Overlap and double-counting audit

- R21 already contains the `22,972,096` prepared-right CSE and
  `66,977,792` unused-rotation deletion. They cannot be counted again.
- The `1.000025` scale was evaluated *after* `lambda=0.0075`, so those two
  are directly composable. Its transfer to any trajectory-changing
  candidate must nevertheless be scored jointly.
- The old `11,146`-view opportunity is not live in R21: the persistent
  K129 graph already reduced the steady view surface to the current R21
  ledger. Do not add the historical `2.6--3.0%` projection.
- The original `3.7394448B` rank-49 result was directionally invalid and is
  superseded by `0.5451264B`.
- Direct late-destination savings overlap M214, and the measured
  direct-destination spelling had a negative effective/wall sign. It is not
  credited above.
- Dynamic support compression, K128/K125 deletion, and sparse-O1
  substitutions all change the trajectory/support. Their separately
  measured ratios cannot be multiplied.
- Lambda retuning is exhausted; family optima disagree and add at most
  `0.016--0.022%`.

## Complete recent K129 disposition

**Banked/promoted:** R21 lambda `0.0075`; its CSE/rotation cleanup; output
scale `1.000025` as a small unimplemented composable lead.

**Numerically interesting but insufficient:** keep-120 dynamic compression;
across-basis dispersion correction; corrected bilinear stack; impossible
reshape ceiling; dead-gamma cleanup.

**Closed by accuracy/family reversal:** K125/K128 deletion, fixed O0/O1
swaps, mixed and balanced SVD frames, lambda retuning, all existing
late/final keep captures, selective-width profiles, rolled mean fold,
upstream covariance innovation, O0 moment surrogate, sparse O1 Edgeworth,
moment-selected O1 supports, isolated supervised basis 42, O1 shadow widths,
and direction thinning.

**Closed by economics or unavailable state:** free batching, generic
prediction-preserving `>=5B` CSE, corrected rank-49, gauge step-20 bridge,
sparse-O1 packing after its accuracy candidates failed, and compact-file
κ3 distillation without the exact public pair-residual assets.

## Recommendation

Run only the fixed depth-selective late-width falsifier first. It is the
sole cheap current-capsule experiment whose exact count is already large
enough to cross `1.2e-7`. If it fails the tiny two-family gate, do not scan
neighboring widths; move to the pair-residual-teacher class or a genuinely
new observable.
