# Projected total contraction selected-support P3/R1

Date: 2026-08-08

Evidence class: **component capacity bound** on Full640--641 and
Generated88--89.  It is intentionally unavailable-output because it reuses
the prior exact-source-selected `S=16` supports.  It is not a production,
physical, broad-statistical, projected-score, or remote receipt.

## Purpose

P1 rejected raw projected audits for random supports, and P2 showed that the
unit-amplitude local-GP residual does not reduce the relevant variance.  This
stage gives contraction estimation its strongest fair test by assuming the
support-selection problem has already been solved: reuse the prior per-row
target-source-selected `S=16` support, then evaluate audit variance after
several increasingly optimistic calibrations.

## Frozen inputs

- exact grouped-basis capture and local-GP fidelity arrays already hash-pinned
  by P1/P2;
- weighted-basis oracle:
  `runtime/artifacts/weighted_basis_source_w1_r1_targetfree_20260808.npz`;
- P1 and P2 sealed arrays.

All files must be target-free seals.  No expectation target, Mini100 row,
physical benchmark, package, upload, or remote action may be opened.

## Methods

For each prior exact-source-selected per-row `S=16` support, reconstruct the
exact projector `P_A` and record its clean residual.  In the projected
coordinate system compare:

1. `raw`: no control variate;
2. `unit_cv`: subtract the sealed local-GP basis prediction;
3. `candidate_scalar`: fit one scalar from the 16 evaluated candidate source
   and prediction pairs, then apply it to complement predictions;
4. `oracle_scalar`: fit the best scalar on the unavailable complement itself;
5. `candidate_matrix`: fit a full projected-coordinate linear map from the
   16 evaluated candidate pairs and apply it to the complement;
6. `oracle_matrix`: fit the best projected-coordinate linear map on the
   unavailable complement itself.

The primary local-GP `basis_energy` variant is used.  Center both candidate
and complement coordinate matrices before calibration because finite-
population audit variance depends on contrasts, not the residual mean.
Least squares uses `rcond=1e-8`.

For each method report exact projected finite-population audit variance for
`q=1,2,4,8,12,16`, its ratio to raw, and the integer audit count required to
reach `8.68e-9`.

## Interpretation

- `candidate_scalar` and `candidate_matrix` are lawful calibration capacity
  tests conditional on the unavailable support.
- `oracle_scalar` and `oracle_matrix` are strict optimistic lower bounds and
  cannot be called estimators.
- If every lawful method still requires dozens of audits, contraction
  estimation is independently closed even if support selection were solved.
- If only `oracle_matrix` passes, the remaining problem is another
  high-dimensional unobserved complement map and is not a production lead.
- No target-aware raw-score replay is authorized unless a lawful method is
  near the allowance at `q<=8`.
