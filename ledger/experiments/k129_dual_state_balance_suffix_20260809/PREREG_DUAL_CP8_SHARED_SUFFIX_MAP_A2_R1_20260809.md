# Preregistration: checkpoint-8 shared-suffix response map A2/R1

Date: 2026-08-09

Evidence class: target-free **component capacity oracle** on fixed Full12 and
Generated124.  No benchmark expectation target is used by the primary gate.

## Distinct mechanism

The killed A1 response model fitted q and polar pair outcomes separately from
rank-at-most-64 probe moments.  A2 instead uses the deterministic structure
that A1 discarded: q/p and both antipodal branches all enter the same realised
suffix.

At checkpoint 8, choose the same frozen D3 assignment.  Run only its selected
population to determine the live late supports and endpoint shift.  For oracle
evaluation, propagate both q and p under that common support path, exactly as
in A1.

Construct the mean-gated Jacobian `J` of that realised common suffix from the
complete selected branch population.  Let `U_r` be the leading left singular
vectors of `J`.  Every q/p branch checkpoint state is observed target-free;
use `h U_r` (and, in the stated diagnostic, its coordinatewise square) as the
response features.

Fit one shared multi-output ridge map across both arms and both antipodal
branches.  A line assigned to q exposes both q branches; a line assigned to p
exposes both p branches.  Four basis-block folds prevent a held line's selected
outcome from fitting its own prediction.  Form the branch-level AIPW estimate
of the equal q/p mean.

## Frozen grid

- ranks: 32, 64, 96;
- relative ridge: 1e-4, 1e-3, 1e-2;
- feature spellings: linear; linear plus coordinatewise square for rank 32;
- assignments: D3 and its exact complement;
- production orientation: first byte of SHA-256(float32 weights), low bit.

## Gates

Primary target-free gate, required on both rows:

- MSE to the actual complete q0/polar teacher <= 2.5e-8;
- common-support teacher drift <= 1e-8;
- selected branch preactivations reproduce the direct selected path to
  maximum absolute error <= 2e-5.

The selected configuration is the frozen hash orientation followed by minimum
worst-family teacher discrepancy, with lower rank and stronger ridge breaking
ties.  If no cell passes both rows, close this shared linear/diagonal-quadratic
suffix map and do not run benchmark targets, broad capture, Mini100, physical
metering, packaging, upload, or submission.

A pass licenses a separate post-seal score/cost oracle only.  It is not a
deployable-estimator receipt.
