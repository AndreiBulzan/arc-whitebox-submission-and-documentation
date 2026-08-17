# Complete-polar trajectory student R1 preregistration

Date: 2026-08-05

Evidence sought: **component**.  This screen opens neither challenge targets
nor Mini100.  It uses the already sealed complete q0/polar predictions as a
target-free teacher and the previously captured q0 per-basis trajectory as
student input.

## Prior-work boundary

The killed basis-trajectory R2 student predicted the all-eight-orientation
signed-premean correction.  Output-alias and hidden-mean students predicted
other teachers from aggregate endpoint state.  None predicted the complete
polar-minus-q0 final-output correction from the matched q0 L4/L8 per-basis
trajectory.  This is therefore a new teacher and causal observable, not a
width/epoch retry of a killed head.

## Fixed screen

Align the sealed polar broad rows (`Full 200:299`, `Generated 16:115`) with
the cached q0 basis-trajectory rows.  For every output coordinate, predict

`(polar_output - q0_output) / q0_basis_endpoint_scale`

from four frozen feature sets:

1. the 15 scalar trajectory summaries;
2. the 129 q0 endpoint sequence plus the 15 summaries;
3. q0, transported-L4, and transported-L8 sequences plus summaries;
4. the complete frozen 789-feature trajectory vector.

Use linear ridge, with feature centering/scaling and ridge selected only on
the source family's last 25 whole networks.  Train on its first 75 networks
and evaluate unchanged on the opposite family.  Ridge values are
`{1e-3, 1e-2, 1e-1, 1, 10, 100}`.  Repeat in both family directions.

## Gate

Promote only if one common feature spelling has, in both directions:

- pooled teacher-correction MSE ratio at most `0.50` versus predicting zero;
- at least `60%` of networks improved; and
- row-ratio p95 at most `1.20`.

A pass licenses a nonlinear/student refinement and then a separately sealed
truth-score screen.  A failure kills this complete-polar student spelling;
it does not reopen tuning on Mini100.

