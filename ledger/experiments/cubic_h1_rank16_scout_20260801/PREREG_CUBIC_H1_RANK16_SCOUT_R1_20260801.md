# Cubic repaired-H1 rank-16 scout R1

Date: 2026-08-01

Evidence class: **component**. This is a fast 16-network development
falsifier, not a whole estimator or remote claim.

The exact rank-4/support-8 sparse-cubic construction produced a target-free
pooled MSE ratio of `0.924808`. This scout changes only the representation
rank from 4 to 16. It keeps support 8, the downstream mean-gated pre/post
Jacobian construction, canonical signs, exact sphere moments, and even/odd
GREG estimator unchanged.

The scorer is fixed before targets are opened:

- families: pre, post, and their concatenation;
- prefix sizes: 4, 8, 12, and 16;
- damping: 0.5 and 1.0;
- promotion gate: pooled ratio at most 0.88, at least 10/16 rows improved,
  maximum row ratio at most 1.75.

A failure kills higher rank alone. It does not license coefficient fitting,
row selection, upload, or submission.
