# Project 27.2 low-K pullback R1 preregistration

Evidence sought: **component** accuracy evidence only.  This is not a
FlopScope receipt, a measured-whole row, broad statistical evidence, or a
submission candidate.

## Question

Can downstream-conditioned input frames bend the low-K variance curve enough
to make a lawful estimator useful at the 27.2B multiplier floor?

The existing low-K experiment propagated only the control frame and a second
fixed rotation.  The existing pullback experiments added 24--68 bases to a
complete K129 control cloud.  Neither experiment propagated a *properly
isolated*, total-K <= 28 portfolio of weight-conditioned frames from layer
zero.  That is the narrow untested intersection measured here.

## Frozen design before targets

- Evaluation weights: the first 16 Full rows and first 16 Generated rows in
  the existing broad endpoint captures.
- Support-selection weights: rows 16 onward in both captures.  No evaluation
  row and no challenge target participates in support selection.
- Frames: control (`q0`), first-layer right Gram (`right`), two-layer
  pullback (`d2`), and two-layer right pullback (`d2right`).
- Nested sizes: 8, 12, 16, 20, 24, 28 bases.
- Each cloud is propagated independently from layer zero with the existing
  H1/H2/L4 and late-width repairs.  Post-hoc endpoint thinning is forbidden.
- Each frame's nested support greedily matches its complete-frame endpoint
  centroid on the disjoint support-selection weights.  This uses weights and
  predictions only, never targets.
- A portfolio with cloud sizes K_i uses the fixed trajectory-proportional
  blend `sum(K_i * prediction_i) / sum(K_i)`.  No fitted coefficient is a
  candidate.

The two primary predeclared K=24 candidates are:

1. `q0:8 + right:8 + d2right:8`
2. `q0:8 + right:8 + d2:8`

All one-, two-, and three-frame allocations with total K <= 28 are reported
as diagnostics.  Selecting the best of that grid after targets are opened is
not itself a frozen deployable candidate.

## Accuracy gates

For a predeclared candidate to keep Project 27.2 alive, require all of:

- Full16 raw MSE <= 1.0e-6;
- Generated16 raw MSE <= 1.0e-6;
- pooled raw MSE <= 9.0e-7;
- finite predictions on every row.

At a true effective compute <= 27.2B, these correspond to <= 1.0e-7 on each
family and <= 9.0e-8 pooled adjusted score.  Passing this component gate does
not establish the compute condition.  A separate capsule-native v0.10.0
physical implementation must subsequently demonstrate total effective
compute, request count, wall time, and lifecycle safety.

## Stop rule

If neither predeclared K=24 candidate passes, do not build the floor-native
runtime from this frame portfolio.  Close this spelling and return to a new
raw-MSE mechanism (connected-state closure / learned premean correction).

