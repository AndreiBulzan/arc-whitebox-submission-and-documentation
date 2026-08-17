# K129 moment-selected O1 subset R1 preregistration

Date: 2026-07-29

Evidence scope: CUDA **component** accuracy acquisition and post-seal
development scoring only. This experiment does not import FlopScope, edit an
estimator, build a package, upload, or submit remotely.

## Question

The prior sparse-O1 experiment used bases `[70,45]`, selected for an endpoint
statistic rather than final raw-moment reconstruction. Can a subset selected
directly from the current m17 arm reproduce its full 17-basis
Edgeworth3433 readout using only one, two, or four isolated O1 trajectories?

## Frozen rows and split

Use the same first eight named rows as the prior sparse-O1 experiment:

```text
Full       [0,1,2,3 | 6,7,8,10]
Generated  [2,4,5,6 | 7,9,10,12]
```

Positions before the bar are the target-free selection split. Positions
after the bar are the held scoring split. Challenge targets are not read
during acquisition or selection.

## Target-free selection

Capture all 17 per-basis final raw moments from the ordinary joint m17
trajectory on all 16 rows. For each subset size `k in {1,2,4}`, exhaustively
search all subsets of the frozen support:

```text
[6,9,11,37,41,42,48,52,55,58,64,65,79,108,111,112,128]
```

For each subset, equally average its per-basis raw moments, apply the fixed
Edgeworth3433 formula, and minimize pooled squared reconstruction error to
the full-17 Edgeworth3433 output on Full positions `0:4` plus Generated
positions `0:4`. No fitted coefficients, challenge targets, target residuals,
or held positions participate in selection. Ties use lexicographic support
order.

## Held execution and score

Because isolated subsets have different H2/L4 repairs and late energy
selections from per-basis slices inside the joint m17 trajectory, propagate
only the selected one-, two-, and four-basis supports independently on held
positions `4:8`.

For each candidate use the frozen spelling

```text
candidate = 1.000025 * (
    q0(lambda=.0075)
    + .0734 * (isolated_O1_Edgeworth3433 - q0(lambda=.0075))
)
baseline = 1.000025 * q0(lambda=.0075)
```

Generated MSE uses the existing label-noise correction. Stop and kill the
lane unless at least one fixed support has:

```text
Full held4 raw ratio       <= .970
Generated held4 raw ratio  <= .970
```

Passing four-row family results remain component diagnostics, not broad
statistical evidence.
