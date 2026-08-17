# Schur pair-stratified complete frames R1

Date: 2026-08-05

Evidence sought: target-free **component** capture followed by broad
statistical scoring on the already sealed Full100 and Generated128 rows.
No Mini100 target, FlopScope, package, upload, or remote action is part of
the discovery pass.

## Prior-work boundary

Prior Schur attempts tested angle-rank alternation, unconstrained energy
partitioning, global Fourier discrepancy colouring, and first-layer marginal
moment descent.  They did not keep the successful one-per-adjacent-angle-bin
stratification while using a target-free tie-breaker inside each bin.

## Frozen candidates

The real-Schur blocks of the q0-to-polar relative rotation are sorted by
absolute angle and paired consecutively.  Exactly one block per pair is put
in each complementary frame.

- `pair_energy_high/low`: choose by first-layer block energy.
- `pair_sensitivity_high/low`: choose by the same energy after weighting
  first-layer neurons by a diagonal mean-field pullback of squared downstream
  sensitivity through layers 1..31.

Existing q0 and `angle_b` predictions are controls.  A new rule promotes
only if its raw ratio is <=0.94 on each family, <=0.92 pooled, improves at
least half the rows in each family, and beats the existing `angle_b` pooled.
One unchanged promoted rule may then be tested once on official Mini100.

