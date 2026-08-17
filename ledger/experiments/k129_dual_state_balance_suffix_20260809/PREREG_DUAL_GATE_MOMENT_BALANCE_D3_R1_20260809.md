# Preregistration: dual exact-gate moment balance D3

Date: 2026-08-09

Evidence sequence: sealed target-free **component** capture on a third disjoint
Full4/Generated4 pilot, then post-seal scoring.  Compute and adjusted-score
figures are **projections**.  No physical FlopScope row, Mini100 prediction,
upload, submission, or remote action is permitted.

## Motivation

D2 showed that 32 exact layer-17 ReLU probes, evaluated from the two complete
checkpoint-16 prefixes, retained 80.6% of the complete dual gain on Full4 and
92.8% on Generated4.  Mean matching alone is therefore informative but below
the roughly 95% retention required by official-Mini economics at about 212B.

D3 tests the narrow covariance-like hypothesis: the missing information is in
second moments of the checkpoint state and exact next-gate activations.  These
features need no additional network propagation once the probes exist.

## Frozen rows and candidates

- Full rows: 87, 97, 107, 117.
- Generated rows: 8, 9, 10, 11.
- Checkpoint: layer 16.
- Probe supports: the 32 or 64 layer-17 columns with largest supplied-weight
  column norm; the all-column arm is diagnostic only.
- One selected existing state per antipodal line; no interpolation.

Candidates:

1. `whole_state_probe_m1_p32` — D2 reference spelling.
2. `whole_state_probe_m12_p32`.
3. `whole_probe_m12_p32` — probe moments only.
4. `cross_state_probe_m12_p32` — exactly one q0 and one polar branch per line.
5. `cross_probe_m12_p32`.
6. `whole_state_probe_m12_p64`.
7. `whole_state_probe_m12_all` — capacity diagnostic, not production eligible.

All feature columns are standardized by their target-free RMS before the fixed
greedy discrepancy rule and two coordinate passes.

## Gates

Promote to a disjoint broad capture only if a production-eligible arm has:

- gain retention at least 0.95 in both families;
- raw ratio no worse than the complete dual raw ratio plus 0.05 in both;
- at least 3/4 rows improved in both; and
- state/probe first-moment relative discrepancy below 1e-4.

If the all-column second-moment diagnostic remains below 0.90 retention in
either family, close checkpoint-state plus one-step-gate moment matching as a
sufficient statistic.  A miss by the 32/64 arms but a pass by all columns is a
dimension/cost failure, not a statistical impossibility result.

