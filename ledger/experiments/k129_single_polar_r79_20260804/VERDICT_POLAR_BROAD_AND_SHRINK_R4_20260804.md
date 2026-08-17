# Polar broad confirmation and output-shrink R4 verdict

Date: 2026-08-04.

The one-trajectory fixed polar frame is killed as a deployment candidate.
After its target-free predictions were sealed on selection-disjoint rows, it
gave raw ratios `0.8860` on Full100 and `1.0180` on Generated100.  Together
with the existing official Mini100 ratio `0.9258` and wide interval, that is
not stable enough to package.

The previously frozen output-space shrink

```text
y = y_q0 + 0.5 * (y_polar - y_q0)
```

is a different result.  The coefficient was selected before Mini100 and
before the new broad capture.  Applied unchanged, it gives:

```text
population       raw ratio     ratio CI95          improved
Full100          0.523917      [0.4755, 0.5731]     94/100
Generated100     0.528728      [0.4498, 0.6174]     90/100
Mini100          0.543034      [0.4863, 0.6015]     98/100
```

This is **broad statistical** confirmation of the accuracy mechanism across
three populations.  It is not a deployment receipt.  The candidate requires
two complete K129 trajectories.  Using the conservative Mini100 ratio and
remote R31 anchor, effective compute must be at most about `246.95B` for an
adjusted `1.10e-7`.  The current K258-class projection is about `273.5B`, so
the remaining blocker is an exact dual-arm graph saving of roughly `26.6B`
effective plus a wall-safe implementation.

Evidence:

- `runtime/artifacts/k129_single_polar_broad_r3_targetfree_20260804.json`
- `runtime/artifacts/k129_single_polar_broad_r3_postseal_20260804.json`
- `runtime/artifacts/k129_polar_output_shrink_broad_r4_postseal_20260804.json`
- `runtime/artifacts/k129_polar_output_shrink_r3_postseal_20260804.json`

No package, upload, submission, or remote action occurred.
