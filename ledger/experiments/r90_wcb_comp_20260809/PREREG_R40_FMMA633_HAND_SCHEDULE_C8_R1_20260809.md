# Preregistration: FMMa633 hand-schedule compatibility C8-R1

Date: 2026-08-09

## Scope

Offline, target-free, exact source reconstruction and cost projection only.
This opens no target, MLP, FlopScope session, benchmark, physical row,
Mini100 row, package, upload, submission, or remote action.

## Inputs

- The live production R40 U/V/W factors reconstructed from the hash-pinned
  R90 ancestry schedule.
- `FMMa633_mul_6_3_3.m` from `jgdumas/Fast-Matrix-Multiplication`, locally
  pinned by repository commit and file SHA-256.

The MATLAB hand schedule is parsed as an exact integer straight-line program.
Its U, V and W output forms must be reconstructed coefficient-for-coefficient.
The complete MATLAB multiplication tensor must equal the ordinary 6x3 by
3x3 matrix-multiplication tensor.  For comparison, normalize one factor by
the live schedule's documented uniform factor-eight scale; that normalized
tensor must equal the live tensor exactly.

## Compatibility classes

Test, in order:

1. literal factor equality after the documented global normalization;
2. common rank permutation plus exact rational per-rank gauges whose U/V/W
   scale product equals the documented global factor eight;
3. tensor equality only.

Only classes 1 or 2 authorize a prediction-association-preserving generated
schedule. Tensor equality alone is still an exact algorithm lead but requires
a separate Mini100 association gate.

## Operation accounting

Record independently per U/V/W axis:

- binary add/subtract nodes;
- nontrivial scalar-multiply nodes;
- explicit standalone unary-negation nodes;
- direct aliases.

For the first static projection, conservatively charge binary, scalar and
standalone-unary nodes equally. Reprice the exact depth-two R40 composition
with mandatory B7 transforms and leaf products held fixed. Each U/W node
weighs 591,192 operations and each V node 7,988 operations over live R90.

Pass the first static gate at at least 3.6B saved and the implementation-
headroom gate at at least 4.5B. A projection never licenses a benchmark.

## Stop conditions

- Reject this source schedule if its tensor is wrong.
- Do not implement if conservative saving is below 3.6B.
- If tensor-correct but not factor-gauge-equivalent, preserve it as an exact
  association-changing candidate and require an explicit implementation and
  official Mini100 plan before any promotion.
