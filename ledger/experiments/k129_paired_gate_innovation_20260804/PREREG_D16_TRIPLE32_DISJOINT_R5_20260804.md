# Preregistration: D16 triple-frame sparse-32 disjoint transfer R5

Evidence sought: a **component** transfer falsifier on estimator outputs that
were not used to select the rule.  Deployment compute and adjusted score stay
**projections**.  This experiment performs no FlopScope row, packaging,
upload, or submission.

## Prior-art boundary

The exact candidate is candidate index 617 in
`compressed_multidepth_scout_r15_postseal_20260802.json`: a matched-q0,
depth-16, ridge-`1e-4`, 32-atom rule drawing from all three alternate frames.
It had a pilot worst score-ratio proxy of `0.686721`, but that index was
chosen after scoring 972 already-sealed candidates on Full16/Generated16.
It is therefore a hypothesis, not broad evidence.

The later broad D16 test was a different target-free rule: one right frame,
24 atoms.  It failed (`0.942130`/`0.996357` raw ratios).  Static coresets,
frame medoids, shallow orientation routers, D6 sparse rules, and balanced
multi-orientation mixtures were also killed.  No artifact on disk applies
the exact D16/all-three/32 matched rule to selection-disjoint networks.

## Frozen spelling

Reconstruct index 617 only from the sealed R14 pilot proxies and the sealed
complete-four-frame endpoint teacher.  Challenge targets are not used to
derive its support or coefficients.  Verify byte-level agreement with the
already-sealed index-617 pilot predictions.

Capture production-screened D16 basis proxies on:

- Full rows `167, 177, ..., 317`;
- Generated rows `16, 17, ..., 31`.

These rows are disjoint from the 16+16 rows used in the R15 search.  Seal the
predictions before opening their targets.

## Decision

Promote to a genuinely broad, selection-disjoint confirmation only if both
families have:

1. pooled raw-MSE ratio to q0 at most `0.82`; and
2. at least `10/16` rows improved.

For sub-`9e-8` economics, the more demanding diagnostic is approximately a
worst-family raw ratio of `0.66` at the R15 projected cost ratio `1.128032`.
That is reported, not used to move the preregistered promotion gate.

