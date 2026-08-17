# Pilot8 remote-fixed official Mini100 P1/R1 preregistration

Date: 2026-08-09

## Frozen candidate

The only candidate is the exact archive:

`tiny_submissions/estimator_best_pilot8_control_remote_eigh_leafalias_meanaxis_importfix_candidate.tar.gz`

SHA-256:

`3ab7bfa4971ef3e3f929772a8de03546dceac0db935709b99e054216df07749d`

Relative to failed submission 327076, it changes only three remote-incompatible
tuple-axis means into reshape plus single-integer-axis means and replaces one
module-level remote float32 constructor with an equivalent Python scalar.
The complete repaired graph is bit-identical to the original over all 8,192
outputs of the sealed local identity diagnostic.

No coefficient, support, frame, control, or output scale may be changed after
this preregistration.

## Evidence sequence

1. Run the exact hash-pinned archive over all 100 official Mini100 weight rows
   using five persistent local full-FlopScope 0.10.0 / WhestBench 0.14.0 lanes.
2. Save predictions, analytical FLOPs, residual time, effective compute and
   hashes without reading `targets.npy`.
3. Seal and hash the complete target-free capture and report.
4. Only then open official Mini100 targets in a separate scoring program.

The full-backend lane is an exact local statistical/count measurement, not a
released-client/server or remote receipt.  The repaired package still requires
a separately authorized remote smoke/evaluation before deployment.

## Fixed scoring

For row `m`, compute final-layer raw MSE `E_m` and

`q_m = max(0.1, C_m / 272e9)`.

The reported adjusted score is `mean(E_m * q_m)`, matching the official
per-row score aggregation.

## Gates

- Hard validity: 100/100 finite predictions, no failures, exact frozen archive
  hash and official Mini100 hash, and every row below 272B effective compute.
- Useful successor: adjusted Mini100 below the current R94 count-only
  projection `1.1146386484507326e-7`.
- Strong checkpoint: adjusted Mini100 at or below `1.10e-7`.
- Goal checkpoint: adjusted Mini100 at or below `8.0e-8`.

If it misses the R94 projection, retain it only as a low-compute diagnostic and
do not tune against Mini100.  Any further estimator change requires a new
target-free mechanism and a new preregistration.

## Action boundary

This preregistration authorizes only local capture and post-seal scoring.  It
does not authorize an upload or submission.
