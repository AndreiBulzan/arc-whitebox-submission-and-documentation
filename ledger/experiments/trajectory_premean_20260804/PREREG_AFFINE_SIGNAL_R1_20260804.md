# Trajectory premean affine-signal R1 preregistration

Evidence sought: **component** only.  No FlopScope, physical row, challenge
target, package, upload, or submission is involved.

Before training a trajectory model, test the cheapest causal observable
already present in the eight-orientation caches.  For orientation zero, use
the runtime-available observer-basis mean `H_l`, analytic observer mean `A_l`,
response map `R_l`, and proxy `p` to form

`affine_l = p + (mean_b(H_l) - A_l) @ R_l`.

The evaluation teacher is the mean final preactivation across all 8 x 129
cached basis trajectories.  The baseline is the orientation-zero 129-basis
mean.  This teacher is not an official challenge target and remains noisy;
only error ratios are interpreted.

For each of the two observer layers:

1. fit one scalar blend coefficient on both Full strata and apply it without
   refitting to Generated;
2. fit on Generated and apply without refitting to both Full strata.

Gate the trajectory-learning lane open only if both cross-family ratios are
`<= 0.80` with at least half of rows improved.  A ratio `<= 0.60` is the
strong promotion threshold from the breakthrough review.  Failure means the
affine aggregate alone is not the causal feature; any later model must use
the per-basis trajectory rather than renamed aggregate summaries.

