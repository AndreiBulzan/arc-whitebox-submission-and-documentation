# K129 sparse supervised O1 correction R1 — preregistration

Date: 2026-07-29.

Evidence scope: ordinary CUDA **component** acquisition and post-seal
development scoring.  This lane does not import FlopScope, edit a production
estimator, build a package, upload, or submit.

## Question

Can the single actual orientation-1 basis `42` recover enough of the useful
full-17 Edgeworth3433 signal when one nonnegative global correction
coefficient is learned only from whole Full MLPs?

This differs from the two closed neighboring experiments:

- fixed supervised basis weights changed the final-ReLU quadrature weights
  of the complete K146 estimator and converged back to uniform;
- moment-selected sparse O1 chose equal-weight supports target-free to
  reconstruct the full-17 raw moments and failed across families.

R1 instead validates the single basis `42` surfaced by the preceding
four-train/four-held/eight-Generated scout.  The validation positions below
are disjoint from every scout position.  No further support search is allowed
before this fixed candidate is scored.

## Frozen banks and target boundary

Use the already frozen endpoint-grid order, but avoid every position used by
the preceding eight-row scouts:

```text
Full train       endpoint-grid positions 16:48    32 MLPs
Full held        endpoint-grid positions 48:64    16 MLPs
Generated guard  endpoint-grid positions 16:48    32 MLPs
```

Execute basis `42` as its own actual one-basis O1 trajectory on all three
banks before opening any validation target.  Only Full-train targets may
then be read to fit the final scalar coefficient.  Serialize the support,
coefficient, Full-held predictions, Generated predictions, indices, and
hashes, and only then open Full-held or Generated targets.

## Fixed statistic

Apply the existing nonlinear Edgeworth3433 readout to the four final raw
moments of the isolated basis-42 trajectory, and form

```text
candidate_42 = 1.000025 * (
    q0(lambda=.0075) + beta_42 * (Edgeworth3433(raw_42) - q0(lambda=.0075))
)
baseline = 1.000025 * q0(lambda=.0075)
```

`beta_42` is one MLP-independent and coordinate-independent scalar constrained
to `[0, 0.15]`.  There is no intercept, row/family identifier, weight-derived
feature, output-coordinate coefficient, or post-held tuning.

## Full-train coefficient

Use the fixed ridge `0.01`, expressed as a multiplier on the Full-train
`X'X`, and fit exactly

```text
X = Edgeworth3433(raw_42) - q0(lambda=.0075)
y = target / 1.000025 - q0(lambda=.0075)
beta_42 = clip(X'y / (1.01 * X'X), 0, .15)
```

on all 32 Full-train MLPs.  There is no ridge grid, cross-validation
selection, or refit after held targets are opened.

## Promotion gate

The exact actual basis-42 trajectory must satisfy both:

```text
Full-held pooled raw ratio                 <= .965
Generated pooled noise-corrected ratio     <= .965
```

All predictions must be finite and nonnegative.  Report observed row-ratio
quantiles but do not select on them.

Stop immediately and do not search another support if either central ratio
exceeds `.97`.  Passing remains component development evidence.  Incremental
count is only a projection of roughly `0.95B` for the actual O1 basis until a capsule-native
FlopScope implementation and complete physical receipt exist.
