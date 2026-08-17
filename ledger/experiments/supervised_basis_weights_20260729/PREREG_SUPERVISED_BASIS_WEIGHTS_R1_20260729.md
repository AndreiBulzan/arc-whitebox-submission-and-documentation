# Supervised fixed K146 basis weights R1 — preregistration

Date: 2026-07-29.

Scope: cache-only component experiment.  It uses existing complete per-basis
final-ReLU responses; it performs no CUDA replay, FlopScope run, estimator
edit, package action, network access, upload, or submission.

## Question and fixed statistic

Can one MLP-independent convex weight vector improve the already-computed
K146 final response enough to matter on whole-MLP holdouts?

The 146 columns are the complete 129-basis orientation-0 response followed
by the current production support from orientation 1:

```text
6, 9, 11, 37, 41, 42, 48, 52, 55, 58, 64, 65, 79, 108, 111, 112, 128
```

The control is the literal production weighting, which is exactly uniform
over those 146 columns.  The candidate is a single nonnegative vector
`w`, shared by every MLP and output coordinate, with `sum(w) = 1`.  Thus it
is exact for constants, has no intercept, row/family identifier, dynamic
feature, or output-coordinate parameter.

For each fixed ridge value, fit the convex problem

```text
minimize_w mean((B w - target)^2)
           + ridge * mean(diag(G_centered)) * ||w - uniform||^2
subject to w >= 0 and sum(w) = 1,
```

where `B` is the 146-column final-ReLU basis response and `G_centered` is
the Gram of `B - mean_basis(B)`.  Solve by deterministic accelerated
projected gradient.  The only ridge grid is

```text
0, 0.001, 0.01, 0.1, 1, 10, 100
```

Select the smallest Full-development MSE ratio, breaking ties toward the
larger ridge.  Refit once with that ridge on Full train plus development.

## Whole-MLP split and target boundary

Use the existing Full1000 corpus:

```text
Full held         index mod 5 == 0       200 MLPs
Full development  index mod 5 == 1       200 MLPs
Full train        index mod 5 in 2,3,4   600 MLPs
```

Only Full train/development target values may be read during fitting and
selection.  Serialize and hash the weights, Full-held predictions, and all
Synthetic1024 predictions before opening Full-held or Synthetic targets.
Synthetic1024 is the process-separated family guard; none of its targets may
enter fitting or selection.

## Promotion gate

After verifying the prediction seal, score observed final-layer MSE and
whole-row ratios separately:

```text
Full-held pooled candidate/control raw ratio       <= 0.94
Synthetic pooled noise-corrected ratio             <= 0.94
Full-held observed row-ratio p95                    <= 1.25
Synthetic observed row-ratio p95                    <= 1.25
all predictions finite and nonnegative
```

The Synthetic correction subtracts the corpus mean final-layer label-noise
MSE from both pooled candidate and control losses before forming the ratio.
Failure of either central family gate kills this spelling immediately.

The readout costs at most a weighted reduction over 146 by 256 values plus
shipping 146 constants; its conservative incremental-count projection is
`0.25B`.  No physical count is claimed.

## Relation to closed work

This is not the wildcard target-free positive cubature: that work selected
tiny supports to approximate a target-free endpoint and found learned
weights worse than equal weights.  R1 retains the full K146 support and
optimizes the challenge final-ReLU target with strict MLP holdouts.

This is also not the nonlinear large-data basis decoder.  That decoder used
116 row- and coordinate-dependent features and a 1,889-parameter nonlinear
correction.  R1 has only one global 146-vector, is convex, constant-exact,
and cannot condition on the network.

The source corpus predates the current compressed K146 implementation; an
R1 pass would therefore be a component signal requiring a fresh
current-graph association capture.  An R1 failure is sufficient to kill the
fixed-weight class.
