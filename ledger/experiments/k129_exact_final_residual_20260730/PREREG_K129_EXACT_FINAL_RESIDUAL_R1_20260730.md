# K129 exact omitted-final residual R1 — preregistration

Date: 2026-07-30.

Evidence scope: target-free CUDA **component** acquisition followed by one
fixed post-seal Full/Generated development gate. Count is a **projection**.
This lane does not run FlopScope, edit an estimator, build a package, upload,
submit, or take any remote action.

## New mechanism

R21 selects 176 of the 200 layer-30 activation coordinates for the final
product. It restores the omitted 24 coordinates only through their sample
mean. That preserves the final preactivation mean, but discards every
sample-dependent fluctuation in the omitted coordinates before the nonlinear
gamma readout.

R1 restores that missing fluctuation exactly:

```text
S192 = the ordinary energy-selected 192 coordinates
R8   = the eight-coordinate complement

z_exact = M267_level4(h[:, S192], W31[S192])
        + dense(h[:, R8], W31[R8])
```

This is the exact 200-coordinate final product of the already-computed K129
layer-30 cloud. It introduces no new trajectory, fitted coefficient, family
router, Gaussian closure, endpoint polynomial, support selection, or target
dependence.

The primary candidate is:

```text
1.000025 * gamma_readout(z_exact, lambda=0.0075)
```

against the identically scaled R21 final-176 control. A final-192
mean-restored prediction is captured only as a diagnostic; it cannot replace
the primary candidate after targets are opened.

## Prespecified untouched slices

Use positions from the already frozen endpoint-grid order that were not used
by the immediately preceding basis-42 train/held/guard:

```text
Full positions       64:72   -> indices [130,131,132,139,140,141,142,146]
Generated positions  48:56   -> indices [90,91,94,100,102,103,104,106]
```

All three prediction arrays and their indices must be serialized and hashed
before either target member is opened.

## Hard gate

The exact-final candidate passes this tiny falsifier only if:

```text
Full pooled raw-MSE ratio                    <= 0.970
Generated noise-corrected pooled ratio       <= 0.970
maximum of the two central ratios            <= 0.970
all outputs finite and nonnegative
```

Any family reversal or either ratio above `0.970` kills the mechanism
immediately. No coefficient, keep size, output scale, slice, or blend may be
changed after scoring.

## Economics boundary

The current K129 final-M267 slope is `20,829,898` counted operations per
retained coordinate. Moving 176 to 192 costs `333,278,368`. The eight-column
dense complement costs about `0.254B`, with small selection/addition work.
The preregistered conservative incremental-count ceiling is `0.75B`, well
below the task's `~3B` limit.

This is not a physical receipt. A passing accuracy result would license only
a source-static implementation audit and a broader prespecified replay.

