# K129 trajectory true-premean R6 — verdict

Evidence label: **component**.

## Verdict

**Kill this exact supervised self-diagnosis spelling.**  Do not retry the same
789 trajectory features, 128--64 head, or true-premean target with more width,
epochs, or checkpoints.

R6 changed the teacher from the earlier four-frame discrepancy to the actual
signed final-preactivation error.  That did not rescue the observable.  The
earliest checkpoint was already worse on its development family and transferred
worse across families:

| fit direction | development ratio | destination ratio | destination p95 | destination rows improved |
|---|---:|---:|---:|---:|
| Full -> Generated | 1.284851 | 1.309986 | 2.194514 | 16.4% |
| Generated -> Full-B | 1.470583 | not usable | not usable | not usable |

The frozen gate required pooled ratio at most `0.80`, p95 at most `1.40`, and
at least 55% of rows improved.  Full -> Generated misses every condition by a
wide margin.  Longer training only worsened the development ratio.

## Association caveat

The Full-B target association in this receipt is misaligned: its reported
baseline MSE is about `3.855`, so those destination numbers are not evidence.
They are not needed for the decision.  The source-family reversals and the
independent Full -> Generated result already kill R6 decisively.

## What is and is not closed

Closed: predicting the true premean error from the existing ordered q0
per-basis endpoint and transported L4/L8 feature set with this scalar head.

Still open: a genuinely different information source or representation.  In
particular, this verdict does not test compression of the full within-basis
direction state in a Walsh basis; it used Walsh/basis summaries only as
predictor features after propagation.

Receipt:
`runtime/artifacts/k129_trajectory_true_premean_r6_20260806.json`.

