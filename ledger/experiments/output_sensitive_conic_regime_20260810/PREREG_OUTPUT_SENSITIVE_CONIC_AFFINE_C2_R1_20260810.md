# Output-sensitive conic affine-map C2/R1 amendment

Date: 2026-08-10

The sealed C1 capture showed that propagating each regime centroid through
ordinary ReLUs has a very large target-free representation error.  This
amendment changes exactly the assumption implicated by that result: each
regime now owns one **fixed affine tail map** rather than allowing its
centroid to select a new activation region.

For each C1 partition, test two target-free maps:

1. `medoid`: the complete tail gate pattern of the member nearest the regime
   feature centroid;
2. `majority`: the coordinatewise majority tail gate pattern in the regime.

Apply the fixed masks after every supplied-weight multiplication to the
empirical checkpoint mean.  If every member shared one activation cone, this
would equal its exact mean output.  The remaining error measures variation
and state/map covariance inside the proposed quotient.

All rows, handoffs, K values, features, K129 inputs, hashes, evidence labels,
and gates remain those in
`PREREG_OUTPUT_SENSITIVE_CONIC_REGIME_C1_R1_20260810.md`.  C1 targets have
already been opened, so this is a mechanistically motivated post-C1
follow-up, not a virgin confirmation.  Its capture still reads weights only
and is sealed before its own predictions are scored.

Prior-art outcome remains **materially new observable** relative to the
state-only Gaussian-sum negative.  Failure of both fixed-map variants at
`K<=64` closes the tested small-affine-atlas spelling much more strongly than
C1 alone, while not proving that every analytic cone integral is impossible.

