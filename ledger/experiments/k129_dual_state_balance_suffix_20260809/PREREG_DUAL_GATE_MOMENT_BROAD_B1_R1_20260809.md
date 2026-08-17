# Preregistration: frozen dual gate-moment broad B1

Date: 2026-08-09

Evidence sequence: target-free sharded capture on Full84 and Generated100,
followed by post-seal **broad statistical** scoring.  Compute and adjusted-score
figures remain **projections**.  No physical FlopScope row, official Mini100
prediction, upload, submission, or remote action is permitted in B1.

## Frozen rule

- Complete q0 and polar prefixes through layer 16.
- Evaluate the 32 layer-17 ReLU coordinates whose supplied-weight columns have
  largest squared norm.
- For every antipodal Kerdock line, select either both q0 branches or both polar
  branches.
- Choose the 33,024 binary decisions by the frozen greedy discrepancy rule,
  balancing first and second moments of checkpoint states and exact probes.
- Propagate the resulting 66,048 selected states through one exact suffix.

No candidate, support width, checkpoint, objective, or coefficient may be
changed after this preregistration.

## Rows

Use the row lists already frozen in the exact broad endpoint archives, excluding
all 16 rows per family consumed by D1--D4:

- Full rows 167, 177, ..., 997 (84 networks; archive positions 16--99).
- Generated rows 16--115 (100 networks; archive positions 16--115).

These rows have appeared in prior polar investigations, but this D3/D4-frozen
selection rule was not developed or selected on them.  The official Mini100 is
reserved for the later exact-package gate.

## Gates

Promote to a capsule-native FlopScope implementation only if:

1. projected adjusted score at 212.429423672B is <=1.10e-7 in both families;
2. noise-corrected raw ratio to q0 is <=0.60 in both families;
3. at least 59/84 Full rows and 70/100 Generated rows improve;
4. the paired-bootstrap 95% upper endpoint of the raw ratio is <0.75 in both;
5. no target-free state/probe mean discrepancy exceeds 1e-4.

The broad candidate is captured in durable 10-row shards.  Existing valid
shards may be resumed; target access remains forbidden until all 19 shards are
sealed and associated.
