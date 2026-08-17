# K129 R35 float32 diagonal continuation — R22 preregistration

Date: 2026-08-03

Evidence before execution: **component** for the target-free capture and
**projection** for compute.  No FlopScope row, package, upload, or submission
is authorized by this document.

## Prior-art boundary

Capsule searches covered `diag_continue`, `diagonal continuation`,
`float32 continuation`, `float64 continuation`, and `closure float32`.
Recorded D4/D5, paid-anchor, q0-anchor coreset, direction-thinning, and
trajectory-merging experiments change the statistic.  No recorded experiment
changes only the arithmetic dtype of the selected R35 depth-6 diagonal
continuation.

## One fixed change

Retain R35's fixed mixed48 support, q0-core64 support and affine weights,
three frames, 256 directions, repaired H2 cloud, layers 2--5, and output
scale.  Cast the q0 and alternate depth-6 mean/variance states to float32 and
run only layers 6--31 of the diagonal Gaussian continuation in float32.

The target-free Full100 and Generated128 predictions are sealed before the
challenge targets are opened.  There is no dtype mixture, coefficient refit,
support search, scale search, or family-specific choice.

## Gates

Promote to one physical whole-row measurement only if:

1. all predictions are finite;
2. each family's raw MSE is no more than `1.002` times R35's corresponding
   sealed raw MSE; and
3. the maximum prediction displacement from R35 is at most `2e-5`.

The physical successor must then demonstrate a counted saving of at least
`0.50B`, stay below the existing result-size cap, and preserve repeatability.
Adjusted-score statements remain projections until that receipt exists.
