# Preregistration: Kerdock-on-Kerdock packet-noise frame R1

Date frozen: 2026-08-08, before target-bearing scoring

Evidence class: `component`

## Question

Does global angular balancing of the one-pair-per-line Gaussian packet
noises recover useful packet gain when local cross-centre K2 matching does
not?

This is the distinct no-feature product-design arm in
`docs/MATCHED_PACKET_COUPLING_RESEARCH_REPORT_20260808.md`.  It does not use
the failed K2 teacher pairs and does not infer or compress a deep state.

## Frozen rows and packet

- Full rows: `640,641`.
- Generated rows: `88,89`.
- Packet radius: `epsilon=0.20`, with the already sealed `rho`, `tau`, and
  exact homogeneous radial normalization.
- Kerdock lines: the capsule-native 33,024-line reconstruction already used
  by the packet and K2 captures.
- Randomized replicates: `32` per arm and network.
- Seed: `2026080841`, with deterministic arm/draw derivations.

No target, truth, residual, or benchmark expectation may be opened by the
capture program.  Predictions and target-free marginal diagnostics must be
sealed before a separate scorer opens the fixed truths.

## Arms

Each arm propagates exactly one antipodal candidate pair per Kerdock line,
or 66,048 rows per draw.

1. `iid_gaussian`: independent standard-Gaussian noise per line.  This is
   the ordinary one-pair packet baseline.
2. `iid_direction_lhs_radius`: independent uniform spherical directions,
   with a randomized Latin-hypercube `chi_256` radius assignment.  This
   isolates radial stratification.
3. `kerdock_natural_lhs_radius`: one global Haar rotation of a second copy
   of the Kerdock lines, natural line-to-line assignment, and randomized
   Latin-hypercube `chi_256` radii.
4. `kerdock_random_lhs_radius`: the same rotated Kerdock noise frame and
   radii, with an independent target-blind line permutation.

For every fixed centre and every arm, the assigned noise must have the exact
`N(0,I_256)` marginal under the design randomization.  The capture must
record radial first/second-moment checks and direction-norm checks but must
not claim that a finite diagnostic proves the distributional identity.

## Target-free capture

For every family, network, arm, and draw, save the 256-dimensional
pair-averaged final prediction after the sealed packet radial normalization.
Also save paired draw identifiers so post-seal comparisons use the common
randomization structure.

The capture may use CUDA for research throughput but performs no FlopScope
run, physical benchmark row, packaging, upload, or submission.  Timing is
diagnostic only.

## Post-seal metrics

For each arm report:

- expected raw MSE averaged over its 32 randomizations;
- packet-gain retention relative to the frozen canonical and ideal-packet
  values;
- Full2, Generated2, and pooled results;
- number of rows whose expected arm MSE beats canonical;
- paired draw-level MSE difference against
  `iid_direction_lhs_radius`, with mean, standard error, and z statistic.

## Frozen gates

The primary production-capacity gate is evaluated separately for each
Kerdock arm.  An arm passes only if all hold:

1. pooled ideal-packet gain retention is at least `0.70`;
2. Full2 retention is at least `0.60`;
3. Generated2 retention is at least `0.60`;
4. at least three of four rows have expected MSE below canonical.

A weaker mechanism-only signal requires, for one frozen Kerdock arm:

1. pooled expected MSE below `iid_direction_lhs_radius`;
2. Full2 and Generated2 expected MSE both below that control;
3. at least three of four rowwise expected MSE values below the control;
4. paired pooled draw-difference z statistic at most `-2` when difference is
   defined as Kerdock minus control.

Decision labels:

- `pass_global_kerdock_packet_capacity` if a primary gate passes;
- `positive_global_balance_below_capacity` if no primary gate passes but a
  weaker mechanism gate passes;
- `kill_kerdock_on_kerdock_noise_frame` otherwise.

No production implementation is licensed by this four-row component test.
A pass licenses a broader frozen-family validation only.

## Interpretation boundaries

- A failure closes the report's random/natural global Kerdock noise-frame
  spellings at this packet radius; it is not a theorem about every dependent
  Gaussian design.
- A radial-control win without a Kerdock-over-radial-control win is evidence
  for radial stratification, not Kerdock angular balance.
- The preceding K2 matched-coupling failure is not reused as a target or
  selection signal here.
