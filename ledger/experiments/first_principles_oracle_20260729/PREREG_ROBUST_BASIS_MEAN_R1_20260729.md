# Robust per-basis mean R1 preregistration

Date: 2026-07-29

Evidence scope: offline component falsifier only.  No FlopScope, physical
row, packaging, upload, submission, or remote action.

## Hypothesis

The production K146 signed-final-preactivation estimate averages 129 complete
Kerdock-basis estimates in orientation 0 and 17 selected basis estimates in
orientation 1.  If the remaining quadrature error is dominated by a small
number of heavy-tailed basis estimates, a target-free robust location
functional may reduce it without changing propagation work.

Freeze the following coordinatewise rules before target access:

- symmetric trimmed means, dropping `2,4,8,16` values from each end of O0;
- symmetric trimmed means, dropping `1,2,3,4` values from each end of O1;
- every O0/O1 trim pair above, blended by the literal `129:17` support
  weights;
- median-of-block-means for O0 with `3,5,9` consecutive deterministic blocks,
  and O1 with `3,5` blocks, again literal-support blended.

No fitted coefficient, target-dependent selection, row routing, or
family-specific rule is allowed.

## Gate

Advance only if one identical rule on both families has:

- pooled exact signed-preactivation MSE ratio `<=0.75` on Full and Generated;
- row-ratio p95 `<=1.05` on both;
- at least 60% of rows improved on both.

This is deliberately demanding: a smaller signed-only gain cannot plausibly
fund a first-place estimator after final-ReLU transfer and implementation
risk.

