# Preregistration: broad K24/K28/K32 floor curve R1

Date: 2026-08-09

## Question

Does the already-defined, target-free nested low-K O0 cloud become accurate
enough at K=24, 28, or 32 to exploit the per-row 0.1 compute floor on the
256-network public development bank?

This is an operating-point measurement, not a new estimator.  The supports,
endpoint correction, output scale, network rows, and K values are frozen
before target access.

## Prior-art preflight

Queries covered `lowk`, `low-K`, `K24`, `K28`, `K32`, `floor`, `portfolio`,
`pullback`, `analytic closure`, `checkpoint rejuvenation`, and the exact
`capture_lowk` call site.

Nearest evidence:

- `k129_pair_residual_sparse_pilot_20260730/lowk_portfolio_r1_postseal_20260731.json`
  measured K=8..32 on only 16 public pilot networks.
- `project27_lowk_pullback_20260804/lowk_pullback_r1_postseal_20260804.json`
  explored pullback rotations around the same small-bank K24/K28 chassis.
- `k129_pair_teacher_large_r2_20260801/.../isolated_k12_state_pair_teacher256_r2.npz`
  is the only 256-network isolated low-K capture, and it stops at K=12.
- `lowk_connected_shrinkage_20260809` closes several K12 marginal, hidden
  covariance, and analytic-shrinkage corrections; it does not measure the
  broad high-K accuracy curve.

Outcome: **materially new evidence scale and operating point**.  No previous
artifact measures K24/K28/K32 on this 256-network bank.  No claim of a novel
estimator mechanism is made.

## Frozen capture

- Weight bank and row ordering: the 256-row pair-teacher R2 bank.
- Supports: prefixes of the existing target-free greedy nested support.
- K values, in order: 24, 28, 32.
- Endpoint lambda: inherited literal `0.0075`.
- Output scale: inherited literal `1.000025`.
- Capture contains only target-free predictions, supports, timing, and CUDA
  peak-memory diagnostics.
- No target, teacher output, or score is opened until the capture and its
  receipt have been written and hashed.
- Ordinary CUDA is component evidence.  No FlopScope physical price or
  Mini100 validation is claimed.

## Frozen scoring

After sealing, open only `target_h31` from the aligned teacher artifact.
Report, separately for the fixed 160/48/48 contiguous train/development/held
blocks:

- mean raw MSE;
- row-MSE p50 and p95;
- wins versus K12 where the aligned K12 prediction exists;
- pairwise monotonicity between successive K values.

No candidate is selected by targets.  All three K values are reported.

## Count projections and target ceilings

The only count figures permitted in this experiment are the inherited
single-cloud **projections** from the 16-row portfolio:

- K24: 24.967373253 B, score multiplier 0.1;
- K28: 28.867574769 B, score multiplier about 0.10613;
- K32: 32.767776285 B, score multiplier about 0.12047.

Therefore the approximate raw-MSE ceilings are:

| K | for 1.10e-7 | for 1.00e-7 | for 8.00e-8 |
|---:|---:|---:|---:|
| 24 | 1.10e-6 | 1.00e-6 | 8.00e-7 |
| 28 | 1.04e-6 | 9.42e-7 | 7.54e-7 |
| 32 | 9.13e-7 | 8.30e-7 | 6.64e-7 |

These are target ceilings, not projected receipts.

## Decision rule

- **Direct survivor:** held projected adjusted score at most 1.10e-7 and held
  p95 at most 3.0e-6.  Promote the best K to a target-free correction capture
  and then to a separately hash-pinned physical implementation.
- **Strong survivor:** held projected adjusted score at most 1.00e-7.  Bank it
  as broad statistical evidence and prioritize physical/official-Mini100
  construction.
- **Breakthrough survivor:** held projected adjusted score at most 8.00e-8.
- **Correction survivor:** held projected adjusted score above 1.10e-7 but no
  more than 1.25e-7, with monotone improvement and no p95 explosion.  Permit
  one predeclared target-free correction lane; do not call the direct chassis
  successful.
- Otherwise close the high-K floor chassis on this bank and return to a
  materially new source-level estimator.

No package, upload, or submission is authorized by any outcome.
